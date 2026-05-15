smell_id:primitive_obsession

# Primitive Obsession - Example 10

```elixir
defmodule OrderImporter do
  @moduledoc false

  # The imported order is built from many generic maps and primitives.
  # Customer, money, address, and line items are not modeled explicitly.
  def import(order_id, customer_name, customer_email, total, currency, status, items, address) do
    %{
      id: order_id,
      customer_name: customer_name,
      customer_email: customer_email,
      total: total,
      currency: currency,
      status: status,
      items: items,
      address: address
    }
  end
end

# ...Use examples...

OrderImporter.import(
  "O-100",
  "Bruno",
  "bruno@example.com",
  245.50,
  "USD",
  "paid",
  [%{sku: "A1", qty: 1}],
  %{street: "Main", city: "Austin"}
)

OrderImporter.import(
  100,
  "Bruno",
  :email,
  "245,50",
  :usd,
  true,
  ["A1", 1],
  "Austin, Main"
)
```
