# Example 8 of **Primitive Obsession**

```elixir
defmodule Checkout do
  def finalize_order(customer_name, customer_email, street, city, state, zip_code, country, subtotal, tax, shipping_fee, discount, currency) do
    total = subtotal + tax + shipping_fee - discount

    receipt = %{
      customer_name: customer_name,
      customer_email: customer_email,
      address: "#{street}, #{city}, #{state}, #{zip_code}, #{country}",
      subtotal: subtotal,
      tax: tax,
      shipping_fee: shipping_fee,
      discount: discount,
      total: total,
      currency: currency
    }

    if total < 0 do
      {:error, "invalid total"}
    else
      {:ok, receipt}
    end
  end
end

# Use examples

Checkout.finalize_order(
  "Mateus Dias",
  "mateus@example.com",
  "Av. PH Rolfs, 100",
  "Viçosa",
  "MG",
  "36570-000",
  "BR",
  149.90,
  14.99,
  20.0,
  10.0,
  "BRL"
)

Checkout.finalize_order(
  "Mateus Dias",
  "mateus@example.com",
  "Av. PH Rolfs, 100",
  "Viçosa",
  "MG",
  "36570-000",
  "BR",
  149.90,
  14.99,
  20.0,
  500.0,
  "BRL"
)
# {:error, "invalid total"}
```

## Why this is a smell

This function models several important business concepts using only primitive values:

- money is represented as raw `float`s
- address is represented as multiple unrelated `string`s
- currency is represented as a plain `string`
- customer identity is split into loose primitive fields

These values clearly belong to richer domain concepts such as `Money`, `Address`, `Customer`, or `OrderSummary`, but the code keeps everything as separate primitives.

That makes the function harder to understand and easier to misuse:
- floating-point values can introduce precision problems for money
- parameters can be passed in the wrong order
- the function signature becomes long and fragile
- business rules are spread across unrelated primitive variables instead of domain types
