smell_id:primitive_obsession

# Primitive Obsession - Example 01

```elixir
defmodule Billing do
  @moduledoc false

  # amount is a float, currency is a string, and tax mode is another string.
  # These primitives try to represent a richer Money domain.
  def charge(customer_id, amount, currency, tax_rate, tax_mode) do
    subtotal = amount

    total =
      case tax_mode do
        "included" -> subtotal
        "excluded" -> subtotal + subtotal * tax_rate
      end

    %{
      customer_id: customer_id,
      currency: currency,
      total: Float.round(total, 2)
    }
  end
end

# ...Use examples...

Billing.charge("cust_123", 19.99, "USD", 0.07, "excluded")
Billing.charge("cust_123", 19.99, "US Dollars", 7, "yes")
```
