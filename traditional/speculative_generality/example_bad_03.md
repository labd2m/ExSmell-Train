smell_id:speculative_generality

# Example 03

```elixir
defmodule ReportPrinter do
  def print(order, format \ :text) do
    case format do
      :text -> render_text(order)
      :pdf -> render_text(order)
      :html -> render_text(order)
    end
  end

  defp render_text(order) do
    "Order ##{order.id} - total: #{order.total}"
  end
end
```

The `format` option exists only for speculative future use. In practice, all formats render the same text string.
