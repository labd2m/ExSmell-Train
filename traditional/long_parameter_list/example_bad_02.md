smell_id:long_parameter_list

```elixir
defmodule Billing do
  def create_invoice(customer_name, customer_email, street, city, zip_code, plan_name, amount_cents, due_date) do
    %{
      customer: %{
        name: customer_name,
        email: customer_email,
        address: "#{street}, #{city}, #{zip_code}"
      },
      plan: plan_name,
      amount_cents: amount_cents,
      due_date: due_date
    }
  end
end

# Hard to call correctly because several arguments are strings with similar meaning.
Billing.create_invoice(
  "Ana",
  "ana@example.com",
  "Main St",
  "Belo Horizonte",
  "30110-000",
  "pro",
  19900,
  ~D[2026-05-10]
)
```
