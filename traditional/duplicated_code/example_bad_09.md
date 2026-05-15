smell_id:duplicated_code

# Example 09

```elixir
defmodule Payments.Refund do
  def create_refund(order, cents, reason) do
    payload = %{
      order_id: order.id,
      customer_id: order.customer_id,
      amount_cents: cents,
      currency: order.currency,
      reason: reason,
      requested_at: DateTime.utc_now()
    }

    Gateway.post("/refunds", payload)
  end

  def create_chargeback(order, cents, reason) do
    payload = %{
      order_id: order.id,
      customer_id: order.customer_id,
      amount_cents: cents,
      currency: order.currency,
      reason: reason,
      requested_at: DateTime.utc_now()
    }

    Gateway.post("/chargebacks", payload)
  end

  def create_adjustment(order, cents, reason) do
    payload = %{
      order_id: order.id,
      customer_id: order.customer_id,
      amount_cents: cents,
      currency: order.currency,
      reason: reason,
      requested_at: DateTime.utc_now()
    }

    Gateway.post("/adjustments", payload)
  end
end

defmodule Gateway do
  def post(path, payload), do: {:ok, %{path: path, payload: payload}}
end
```
