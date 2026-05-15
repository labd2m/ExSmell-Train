smell_id:use_instead_of_import

# Example 04 - Formatting helpers imported through a DSL-like `use`

The client wants only helper functions, but the library uses `use` to inject them.
This worsens readability and introduces a hidden imported function.

```elixir
defmodule MoneyHelpers do
  def format(cents), do: "$" <> Integer.to_string(cents)
  def currency_symbol, do: "$"
end

defmodule MoneyDsl do
  defmacro __using__(_opts) do
    quote do
      import MoneyHelpers

      def dsl_enabled?, do: true
    end
  end

  def dsl_enabled?, do: true
end

defmodule CheckoutView do
  use MoneyDsl

  def format(_cents) do
    :localized_value
  end

  def receipt_total(cents) do
    format(cents)
  end
end
```

There is no need for a `use` dependency here. `CheckoutView` could simply import
`MoneyHelpers` if that is all it needs, but instead `MoneyDsl` hides the dependency
and causes a local conflict with `format/1`.
