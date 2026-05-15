smell_id:complex_else_clauses_in_with

# Example 06

```elixir
defmodule MyApp.Checkout do
  alias MyApp.{Inventory, Payments, Orders}

  def finalize(order_id, payment_token) do
    with {:ok, order} <- Orders.fetch_pending(order_id),
         {:ok, reserved_order} <- Inventory.reserve(order),
         {:ok, charge} <- Payments.capture(payment_token, reserved_order.total),
         {:ok, completed_order} <- Orders.mark_paid(reserved_order, charge.id) do
      {:ok, completed_order}
    else
      {:error, :not_found} -> {:error, :order_not_found}
      {:error, :already_paid} -> {:error, :invalid_order_state}
      {:error, :out_of_stock} -> {:error, :stock_unavailable}
      {:error, :card_declined} -> {:error, :payment_failed}
      {:error, :gateway_timeout} -> {:error, :payment_timeout}
      {:error, :stale_order} -> {:error, :concurrency_conflict}
    end
  end
end
```

The `else` clause centralizes inventory, payment, and order-state failures. That makes the
checkout flow harder to understand and change safely.
