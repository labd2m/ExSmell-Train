smell_id:inappropriate_intimacy

# Example 04 - Invoice reading subscription and plan internals

```elixir
defmodule Plan do
  defstruct [:name, :monthly_price, :discount_percent]
end

defmodule Subscription do
  defstruct [:user_id, :status, :renewal_day, :plan]
end

defmodule Invoice do
  def generate(%Subscription{} = subscription) do
    if subscription.status == :active do
      price = subscription.plan.monthly_price
      discount = subscription.plan.discount_percent || 0
      due_day = subscription.renewal_day

      %{
        user_id: subscription.user_id,
        due_day: due_day,
        total: price - price * discount / 100
      }
    else
      {:error, :inactive_subscription}
    end
  end
end
```

The invoice module depends on deep knowledge of how `Subscription` and `Plan` store business rules.
