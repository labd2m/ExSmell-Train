smell_id:speculative_generality

# Example 06

```elixir
defmodule PaymentGateway do
  def charge(order), do: StripeGateway.charge(order)
end

defmodule StripeGateway do
  def charge(order) do
    {:ok, %{provider: :stripe, order_id: order.id}}
  end
end

defmodule PaypalGateway do
  def charge(order) do
    {:ok, %{provider: :paypal, order_id: order.id}}
  end
end

defmodule BraintreeGateway do
  def charge(order) do
    {:ok, %{provider: :braintree, order_id: order.id}}
  end
end
```

Multiple gateway modules were introduced for flexibility, but only `StripeGateway` is actually used anywhere.
