smell_id:comments

# Example 01

```elixir
defmodule Billing.InvoiceCalculator do
  # Calculates the final total for an invoice.
  # Expects a list of line items with :price and :quantity.
  # Applies a flat tax percentage received as argument.
  def total(items, tax_percent) do
    subtotal =
      Enum.reduce(items, 0, fn item, acc ->
        acc + item.price * item.quantity
      end)

    # Adds tax to subtotal and returns the resulting amount.
    subtotal + subtotal * tax_percent / 100
  end
end
```
