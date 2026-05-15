smell_id:comments

# Example 03

```elixir
defmodule Shipping.LabelFormatter do
  # Builds a printable address label.
  # Receives an order map and returns a multi-line string.
  # The city and zip code are shown on the last line.
  def format(order) do
    [
      order.recipient,
      order.street,
      "#{order.city}, #{order.state} #{order.zip_code}"
    ]
    |> Enum.join("\n")
  end
end
```
