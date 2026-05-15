smell_id:long_function

```elixir
defmodule Checkout do
  def finalize(cart, user, payment_gateway) do
    if Enum.empty?(cart.items) do
      raise "empty cart"
    end

    available_items =
      cart.items
      |> Enum.filter(fn item -> item.stock >= item.quantity end)

    subtotal =
      available_items
      |> Enum.reduce(0, fn item, acc ->
        acc + item.price * item.quantity
      end)

    shipping =
      cond do
        subtotal > 200 -> 0
        user.country == "US" -> 15
        true -> 40
      end

    discount =
      if user.vip do
        subtotal * 0.2
      else
        0
      end

    total = subtotal + shipping - discount

    payload = %{
      customer_id: user.id,
      currency: cart.currency,
      amount: total,
      items: Enum.map(available_items, fn item -> %{sku: item.sku, qty: item.quantity} end)
    }

    case payment_gateway.charge(payload) do
      {:ok, tx} ->
        Enum.each(available_items, fn item ->
          IO.puts("Reserving #{item.quantity} units of #{item.sku}")
        end)

        IO.puts("Payment successful: #{tx.id}")
        {:ok, %{transaction_id: tx.id, total: total}}

      {:error, reason} ->
        IO.puts("Payment failed: #{inspect(reason)}")
        {:error, reason}
    end
  end
end
```
