smell_id:large_class

# Example 07

```elixir
defmodule Marketplace.OrderCenter do
  alias Market.Reviews
  alias Market.Returns
  alias Market.Invoices

  # Checkout
  def checkout(cart, user) do
    %{cart: cart, user: user, status: :placed}
  end

  # Coupon validation
  def validate_coupon("BLACKFRIDAY"), do: {:ok, 0.3}
  def validate_coupon("VIP"), do: {:ok, 0.15}
  def validate_coupon(_), do: :error

  # Shipping policy
  def shipping_deadline(:express), do: 1
  def shipping_deadline(:standard), do: 5
  def shipping_deadline(_), do: 10

  # Returns
  def open_return(order, item_id) do
    Returns.open(order.id, item_id)
  end

  # Reviews
  def request_review(order) do
    Reviews.request(order.id)
  end

  # Invoice generation
  def issue_invoice(order) do
    Invoices.issue(order)
  end

  # Fraud checks
  def suspicious?(order) do
    order.total > 10_000 or order.country not in ["BR", "US"]
  end

  # Post-order messaging
  def thank_you_message(user) do
    "Thanks for shopping with us, #{user.name}"
  end
end
```
