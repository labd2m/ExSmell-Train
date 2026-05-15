smell_id:feature_envy

# Example 06

```elixir
defmodule InvoiceLine do
  def find(id) do
    %{
      id: id,
      description: "Keyboard",
      quantity: 4,
      unit_value: 90.0,
      tax_rate: 0.18
    }
  end

  def taxable_amount(line), do: line.quantity * line.unit_value
end

defmodule InvoicePrinter do
  def render_line(id) do
    line = InvoiceLine.find(id)
    base = InvoiceLine.taxable_amount(line)
    taxes = base * line.tax_rate
    total = base + taxes

    "#{line.description} x#{line.quantity} => #{total}"
  end
end
```
