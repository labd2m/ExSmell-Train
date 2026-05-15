smell_id:feature_envy

# Example 03

```elixir
defmodule CartItem do
  def load(id) do
    %{
      id: id,
      unit_price: 30.0,
      quantity: 3,
      category: :book,
      taxable: true
    }
  end

  def category_discount(:book), do: 0.05
  def category_discount(_), do: 0.0
end

defmodule Checkout do
  def line_total(id) do
    item = CartItem.load(id)
    base = item.unit_price * item.quantity
    tax = if item.taxable, do: base * 0.12, else: 0
    discount = base * CartItem.category_discount(item.category)

    base + tax - discount
  end
end
```
