smell_id:feature_envy

# Example 04

```elixir
defmodule UserAccount do
  def fetch(id) do
    %{
      id: id,
      email: "lucas@example.com",
      confirmed_at: ~N[2026-01-10 10:00:00],
      inserted_at: ~N[2026-01-01 08:00:00],
      login_attempts: 1
    }
  end

  def confirmed?(%{confirmed_at: nil}), do: false
  def confirmed?(_), do: true
end

defmodule AuditLog do
  def account_summary(user_id) do
    user = UserAccount.fetch(user_id)

    age_in_days =
      NaiveDateTime.diff(~N[2026-01-20 08:00:00], user.inserted_at, :day)

    state =
      if UserAccount.confirmed?(user) do
        "confirmed"
      else
        "pending"
      end

    "#{user.email} is #{state}, account age #{age_in_days} days, attempts #{user.login_attempts}"
  end
end
```
