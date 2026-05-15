smell_id:large_class

# Example 08

```elixir
defmodule Streaming.SubscriptionHub do
  alias StreamApp.Billing
  alias StreamApp.Recommendations
  alias StreamApp.Mailer

  # Authentication / devices
  def sign_in(email, password), do: {:ok, %{email: email, password: password}}
  def sign_out(user), do: {:ok, user}
  def register_device(user, device), do: %{user | devices: [device | Map.get(user, :devices, [])]}

  # Plans
  def upgrade_plan(user, :premium), do: %{user | plan: :premium}
  def downgrade_plan(user, :basic), do: %{user | plan: :basic}

  # Billing
  def charge_monthly(user) do
    Billing.charge(user.customer_id, plan_price(user.plan))
  end

  def plan_price(:basic), do: 19.90
  def plan_price(:premium), do: 39.90
  def plan_price(:family), do: 49.90

  # Recommendations
  def recommend_titles(user) do
    Recommendations.for_user(user.id)
  end

  # Marketing / emails
  def send_trial_ending_email(user) do
    Mailer.send(user.email, "Your trial is about to end")
  end

  # Reports
  def active_device_count(user) do
    user.devices |> Enum.count()
  end
end
```
