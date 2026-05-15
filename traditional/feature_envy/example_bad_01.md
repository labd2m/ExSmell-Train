smell_id:feature_envy

# Example 01

```elixir
defmodule OrderItem do
  def find_item(id) do
    %{
      id: id,
      price: 120.0,
      taxes: 10.0,
      amount: 2,
      coupon: "VIP10"
    }
  end

  def coupon_discount("VIP10"), do: 0.10
  def coupon_discount(_), do: nil
end

defmodule Order do
  def calculate_total_item(id) do
    item = OrderItem.find_item(id)
    subtotal = (item.price + item.taxes) * item.amount
    discount = OrderItem.coupon_discount(item.coupon)

    if discount do
      subtotal - subtotal * discount
    else
      subtotal
    end
  end
end
```
