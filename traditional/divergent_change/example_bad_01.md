smell_id:divergent_change

# Example 01

```elixir
defmodule MyApp.Account do
  @moduledoc """
  This module changes for many unrelated reasons:
  - sign-in policy changes
  - password reset rules
  - subscription billing rules
  - welcome email content changes
  """

  # Reason to change: authentication policies
  def signin_with_password(email, password) do
    user = Repo.get_by(User, email: email)

    cond do
      is_nil(user) -> {:error, :not_found}
      user.locked_at != nil -> {:error, :locked}
      Bcrypt.verify_pass(password, user.password_hash) -> {:ok, user}
      true -> {:error, :invalid_credentials}
    end
  end

  # Reason to change: password reset rules
  def send_reset_email(email) do
    token = Phoenix.Token.sign(MyAppWeb.Endpoint, "reset", email)
    Mailer.deliver(%{
      to: email,
      subject: "Reset your password",
      body: "Use this token: #{token}"
    })
  end

  # Reason to change: billing policies
  def charge_subscription(user_id) do
    user = Repo.get!(User, user_id)
    amount = if user.plan == :pro, do: 29_00, else: 9_00
    BillingGateway.charge(user.customer_id, amount)
  end

  # Reason to change: email copy / onboarding flow
  def send_welcome_email(user) do
    Mailer.deliver(%{
      to: user.email,
      subject: "Welcome",
      body: "Welcome to MyApp, #{user.first_name}!"
    })
  end
end
```
