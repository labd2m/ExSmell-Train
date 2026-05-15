smell_id:speculative_generality

# Example 04

```elixir
defmodule PriceCalculator do
  def total(price, tax, currency \ :usd) do
    base = price + tax

    case currency do
      :usd -> base
      :eur -> base
      :brl -> base
    end
  end
end
```

The function is parameterized by `currency`, but the value does not influence the result at all.
