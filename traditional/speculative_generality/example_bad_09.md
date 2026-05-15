smell_id:speculative_generality

# Example 09

```elixir
defmodule Shipping do
  def calculate(order, region \ :domestic) do
    subtotal = Enum.reduce(order.items, 0, fn item, acc -> acc + item.price end)

    case region do
      :domestic -> subtotal * 0.1
      :international -> subtotal * 0.1
    end
  end

  defp customs_fee(_order) do
    25.0
  end
end
```

The module was designed for future regional rules. Today, both regions use the same formula and `customs_fee/1` is dead code.
