smell_id:using_exceptions_for_control_flow

# Example 07

A payment gateway wrapper raises when a card is declined, and the checkout service rescues to represent a perfectly expected business outcome.

```elixir
defmodule PaymentGateway do
  def charge!(%{number: "4242", amount: amount}) when amount > 0 do
    %{status: :captured, id: "pay_123"}
  end

  def charge!(_params) do
    raise RuntimeError, "card declined"
  end
end

defmodule CheckoutService do
  def pay(card, amount) do
    try do
      payment = PaymentGateway.charge!(%{number: card, amount: amount})
      {:ok, payment}
    rescue
      e in RuntimeError ->
        {:payment_failed, e.message}
    end
  end
end

# Use examples
CheckoutService.pay("4242", 5000)
CheckoutService.pay("4000", 5000)
```
