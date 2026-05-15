smell_id:large_class

# Example 02

```elixir
defmodule Accounts do
  alias MyApp.Mailer
  alias MyApp.Payments
  alias MyApp.Audit

  # Authentication
  def signin_with_password(email, password) do
    # ...
    {:ok, %{email: email}}
  end

  def signin_with_google(google_data) do
    # ...
    {:ok, google_data}
  end

  def reset_password(user) do
    Mailer.send(user.email, "Reset your password")
  end

  # Profile management
  def update_name(user, name) do
    %{user | name: name}
  end

  def update_avatar(user, url) do
    %{user | avatar_url: url}
  end

  # Billing
  def charge_subscription(user, amount) do
    Payments.charge(user.default_card, amount)
  end

  def cancel_subscription(user) do
    Payments.cancel(user.subscription_id)
  end

  # Notifications
  def send_welcome_email(user) do
    Mailer.send(user.email, "Welcome!")
  end

  def send_plan_changed_email(user) do
    Mailer.send(user.email, "Your plan changed")
  end

  # Audit / analytics
  def log_signin(user) do
    Audit.log(:signin, user.id)
  end

  def log_profile_change(user, attrs) do
    Audit.log(:profile_change, {user.id, attrs})
  end
end
```
