smell_id:use_instead_of_import

# Example 09 - Discount helpers injected into pricing code

The pricing module could explicitly import helper functions, but instead it uses a
library that injects them through `__using__/1`.

```elixir
defmodule DiscountHelpers do
  def apply_discount(price, rate), do: price - trunc(price * rate)
  def clamp_to_zero(value) when value < 0, do: 0
  def clamp_to_zero(value), do: value
end

defmodule PricingLibrary do
  defmacro __using__(_opts) do
    quote do
      import DiscountHelpers

      def pricing_library, do: :enabled
    end
  end

  def pricing_library, do: :enabled
end

defmodule ClearancePrice do
  use PricingLibrary

  def apply_discount(_price, _rate) do
    :manual_review
  end

  def final(price) do
    apply_discount(price, 0.25) |> clamp_to_zero()
  end
end
```

The use of `use PricingLibrary` is broader than needed and makes it harder to see
that `clamp_to_zero/1` and `apply_discount/2` come from another module. It also
conflicts with the local `apply_discount/2`.
