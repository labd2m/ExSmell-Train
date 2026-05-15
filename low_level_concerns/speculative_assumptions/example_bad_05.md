smell_id:speculative_assumptions

# Example 05 - Money parser that assumes one decimal separator

```elixir
defmodule PriceTag do
  @doc """
  Returns the cents part of a formatted price.

  The implementation assumes prices always use a single `.` separator.
  """
  def cents(formatted_price) do
    formatted_price
    |> String.split(".")
    |> Enum.at(1)
  end
end

# Planned input
PriceTag.cents("19.99")
# => "99"

# Unplanned input with a thousands separator returns the wrong segment
PriceTag.cents("1.299.99")
# => "299"
```
