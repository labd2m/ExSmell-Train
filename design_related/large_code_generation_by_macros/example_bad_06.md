smell_id:large_code_generation_by_macros
example_id:06
title: Feature-flag macro that emits repeated instrumentation and fallback code

# Example 06

A single macro call creates several function clauses, tracing logic, and default handling. Repeated dozens or hundreds of times, it becomes a large compilation burden.

```elixir
defmodule FlagDSL do
  defmacro __using__(_opts) do
    quote do
      import FlagDSL
      Module.register_attribute(__MODULE__, :flags, accumulate: true)
      @before_compile FlagDSL
    end
  end

  defmacro flag(name, default, opts \\ []) do
    quote do
      name = unquote(name)
      default = unquote(default)
      opts = unquote(opts)

      if not is_atom(name), do: raise ArgumentError, "flag name must be atom"

      @flags {name, default, opts}

      def enabled?(unquote(name), context) do
        source =
          cond do
            Map.has_key?(context, :overrides) and is_map(context.overrides) ->
              Map.get(context.overrides, unquote(name), unquote(default))

            Keyword.get(unquote(opts), :env_var) ->
              case System.get_env(Keyword.get(unquote(opts), :env_var)) do
                "true" -> true
                "false" -> false
                _ -> unquote(default)
              end

            true ->
              unquote(default)
          end

        if Keyword.get(unquote(opts), :trace, false) do
          send(self(), {:flag_evaluated, unquote(name), source})
        end

        source
      end

      def explain(unquote(name), context) do
        %{
          name: unquote(name),
          enabled: enabled?(unquote(name), context),
          default: unquote(default),
          source: if(Map.has_key?(context, :overrides), do: :override, else: :default)
        }
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def enabled?(_name, _context), do: false
      def explain(name, _context), do: %{name: name, enabled: false}
      def all_flags, do: @flags
    end
  end
end

defmodule Features do
  use FlagDSL

  flag :new_dashboard, false, trace: true
  flag :fast_checkout, false, env_var: "FAST_CHECKOUT"
  flag :beta_search, true
  flag :show_recommendations, true
  flag :enable_csv_exports, false
end
```