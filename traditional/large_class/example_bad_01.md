smell_id:large_class

# Example 01

```elixir
defmodule Shop.CartManager do
  alias Shop.Location
  alias Shop.Notifier
  alias Shop.InvoicePrinter

  # Pricing rules
  def calculate_total(items, subscription) do
    subtotal =
      items
      |> Enum.map(&(&1.price * &1.quantity))
      |> Enum.sum()

    apply_discount(subtotal, subscription)
  end

  # Shipping rules
  def calculate_shipping(zip_code, %{tier: :gold}), do: 0.0
  def calculate_shipping(zip_code, %{tier: :platinum}), do: 0.0
  def calculate_shipping(zip_code, _subscription) do
    12.5 * Location.distance_factor(zip_code)
  end

  # Discount rules
  def apply_discount(total, %{tier: :gold}), do: total * 0.95
  def apply_discount(total, %{tier: :platinum}), do: total * 0.90
  def apply_discount(total, _), do: total

  # Coupon rules
  def apply_coupon(total, "WELCOME10"), do: total - 10
  def apply_coupon(total, "SHIPFREE"), do: total
  def apply_coupon(total, _), do: total

  # Inventory rules
  def reserve_items(order) do
    Enum.each(order.items, fn item ->
      :ets.update_counter(:inventory, item.sku, {2, -item.quantity})
    end)
  end

  # Notification rules
  def send_checkout_email(user, order) do
    Notifier.deliver(user.email, "Thanks for your order ##{order.id}")
  end

  # Reporting rules
  def print_invoice(user, order) do
    InvoicePrinter.print(user, order)
  end
end
```
