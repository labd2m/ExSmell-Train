smell_id:large_code_generation_by_macros
example_id:05
title: Query DSL that expands whole filtering and sorting pipelines at compile time

# Example 05

Every `filter/3` macro invocation emits substantial code. A reporting module with many filters becomes heavy to compile and hard to reason about.

```elixir
defmodule QueryDSL do
  defmacro __using__(_opts) do
    quote do
      import QueryDSL
      Module.register_attribute(__MODULE__, :filters, accumulate: true)
      @before_compile QueryDSL
    end
  end

  defmacro filter(name, field, predicate) do
    quote do
      name = unquote(name)
      field = unquote(field)
      predicate = unquote(predicate)

      if not is_atom(name), do: raise ArgumentError, "name must be atom"
      if not is_atom(field), do: raise ArgumentError, "field must be atom"

      @filters {name, field, predicate}

      def apply_filter(items, unquote(name), value) do
        items
        |> Enum.map(fn item ->
          raw_value =
            case item do
              %{unquote(field) => v} -> v
              _ -> nil
            end

          {item, raw_value}
        end)
        |> Enum.filter(fn {_item, raw_value} ->
          case unquote(predicate) do
            :eq -> raw_value == value
            :gt -> is_number(raw_value) and raw_value > value
            :lt -> is_number(raw_value) and raw_value < value
            :contains -> is_binary(raw_value) and String.contains?(raw_value, to_string(value))
            _ -> false
          end
        end)
        |> Enum.map(fn {item, _raw_value} -> item end)
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def apply_filter(items, _name, _value), do: items
      def known_filters, do: @filters
    end
  end
end

defmodule ReportQueries do
  use QueryDSL

  filter :by_status, :status, :eq
  filter :by_min_total, :total, :gt
  filter :by_max_total, :total, :lt
  filter :by_customer_name, :customer_name, :contains
  filter :by_region, :region, :eq
  filter :by_currency, :currency, :eq
end
```