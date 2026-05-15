smell_id:use_instead_of_import

# Example 03 - Query predicates hidden behind `use`

This module only needs predicate helpers. Using `use` injects a broader dependency
than necessary and hides where `eq/2` and `gt/2` come from.

```elixir
defmodule QueryPredicates do
  def eq(field, value), do: {:eq, field, value}
  def gt(field, value), do: {:gt, field, value}
end

defmodule MiniQuery do
  defmacro __using__(_opts) do
    quote do
      import QueryPredicates

      def from_query_lib do
        :ok
      end
    end
  end

  def from_query_lib, do: :ok
end

defmodule SalesFilter do
  use MiniQuery

  def gt(_field, _value) do
    {:custom_operator}
  end

  def expensive_orders do
    [eq(:status, :paid), gt(:total, 1000)]
  end
end
```

A reader has to inspect `MiniQuery.__using__/1` to understand that it imports
`QueryPredicates`. The dependency is implicit, and the local `gt/2` collides with
an imported helper.
