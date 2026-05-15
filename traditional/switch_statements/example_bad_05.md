smell_id:switch_statements

# Example 05

```elixir
defmodule PaymentGatewayRouter do
  def gateway(:visa), do: StripeGateway
  def gateway(:mastercard), do: StripeGateway
  def gateway(:amex), do: AdyenGateway
end

defmodule PaymentFees do
  def fee(:visa), do: 0.03
  def fee(:mastercard), do: 0.03
  def fee(:amex), do: 0.05
end

defmodule PaymentLabels do
  def label(:visa), do: "Visa"
  def label(:mastercard), do: "Mastercard"
  def label(:amex), do: "American Express"
end

defmodule Checkout do
  def charge(amount, card_brand) do
    gateway = PaymentGatewayRouter.gateway(card_brand)
    fee = PaymentFees.fee(card_brand)

    gateway.charge(amount + amount * fee, PaymentLabels.label(card_brand))
  end
end
```

The same card brand branching is repeated in multiple places. Supporting a new brand would mean editing several different switch statements.
