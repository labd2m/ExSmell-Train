# Example 6 of **Accessing non-existent Map/Struct fields**

```elixir
defmodule Shipping do
  def shipping_label(order) do
    customer = order[:customer]
    address = order[:shipping_address]

    recipient = customer[:name]
    street = address[:street]
    city = address[:city]
    state = address[:state]
    postal_code = address[:postal_code]
    country = address[:country]

    destination =
      if country == "BR" do
        "#{city}/#{state}"
      else
        "#{city}, #{country}"
      end

    %{
      recipient: recipient,
      line1: street,
      line2: destination,
      postal_code: postal_code
    }
  end
end

# Use examples

order_ok = %{
  customer: %{name: "Mateus"},
  shipping_address: %{
    street: "Av. PH Rolfs, 100",
    city: "Viçosa",
    state: "MG",
    postal_code: "36570-000",
    country: "BR"
  }
}

order_missing_country = %{
  customer: %{name: "Mateus"},
  shipping_address: %{
    street: "Av. PH Rolfs, 100",
    city: "Viçosa",
    state: "MG",
    postal_code: "36570-000"
  }
}

Shipping.shipping_label(order_ok)
# %{
#   recipient: "Mateus",
#   line1: "Av. PH Rolfs, 100",
#   line2: "Viçosa/MG",
#   postal_code: "36570-000"
# }

Shipping.shipping_label(order_missing_country)
# %{
#   recipient: "Mateus",
#   line1: "Av. PH Rolfs, 100",
#   line2: "Viçosa, ",
#   postal_code: "36570-000"
# }
```

## Why this is a smell

The function uses dynamic map access such as `order[:customer]` and `address[:country]`.  
If a key does not exist, Elixir returns `nil` instead of failing immediately.

That makes the result ambiguous:
- did `country` really exist with a `nil` value?
- or was the key missing entirely?

In this example, a missing `:country` silently produces a malformed shipping label instead of exposing the bug early. This harms readability, debugging, and reliability.
