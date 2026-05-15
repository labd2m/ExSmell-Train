smell_id:feature_envy

# Example 10

```elixir
defmodule SubscriptionPlan do
  def find(id) do
    %{
      id: id,
      monthly_price: 39.0,
      seats: 8,
      extra_seat_price: 12.0,
      included_seats: 5
    }
  end

  def extra_seats(plan) do
    max(plan.seats - plan.included_seats, 0)
  end
end

defmodule SubscriptionCheckout do
  def charge_amount(plan_id) do
    plan = SubscriptionPlan.find(plan_id)
    extras = SubscriptionPlan.extra_seats(plan)
    extra_cost = extras * plan.extra_seat_price

    plan.monthly_price + extra_cost
  end
end
```
