smell_id:comments

# Example 08

```elixir
defmodule Payments.FeeCalculator do
  # Computes the service fee charged on a purchase.
  # Amounts above 1000 pay a reduced percentage.
  # Returns the numeric fee without rounding.
  def fee(amount) when amount > 1000 do
    amount * 0.015
  end

  # Computes the service fee for smaller purchases.
  # Uses the default rate for normal transactions.
  def fee(amount) do
    amount * 0.03
  end
end
```
