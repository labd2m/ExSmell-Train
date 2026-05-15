smell_id:complex_extractions_in_clauses

# Example 02 - Order shipping rules with too much extraction in the clause head

```elixir
defmodule Shop.Order do
  defstruct [:id, :status, :total_cents, :country, :carrier, :priority]
end

defmodule Shipping do
  alias Shop.Order

  def dispatch(%Order{
        id: id,
        status: status,
        total_cents: total_cents,
        country: country,
        carrier: carrier,
        priority: priority
      })
      when status == :paid and total_cents > 20_000 and country in ["US", "CA"] do
    {:express, "Order #{id} goes with #{carrier} as #{priority} priority"}
  end

  def dispatch(%Order{
        id: id,
        status: status,
        total_cents: total_cents,
        country: country,
        carrier: carrier,
        priority: priority
      })
      when status == :paid and total_cents <= 20_000 and country in ["US", "CA"] do
    {:standard, "Order #{id} goes with #{carrier} as #{priority} priority"}
  end

  def dispatch(%Order{
        id: id,
        status: status,
        total_cents: total_cents,
        country: country,
        carrier: carrier,
        priority: priority
      })
      when status in [:pending, :cancelled] do
    {:blocked, "Order #{id} for #{country} cannot ship via #{carrier}; total=#{total_cents}; priority=#{priority}"}
  end
end
```

The clause head is doing too much: some fields are extracted for guard checks, and many others are only interpolated in the returned message.
