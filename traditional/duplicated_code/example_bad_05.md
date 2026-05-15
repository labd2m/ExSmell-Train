smell_id:duplicated_code

# Example 05

```elixir
defmodule Notifications.Email do
  def send_welcome(user) do
    subject = "Welcome, #{user.first_name}"
    body =
      "Hello #{user.first_name},\n\n" <>
        "Your account has been created.\n" <>
        "Email: #{user.email}\n" <>
        "Support: support@example.com\n"

    Mailer.deliver(user.email, subject, body)
  end

  def send_password_reset(user, token) do
    subject = "Reset your password"
    body =
      "Hello #{user.first_name},\n\n" <>
        "Use this token to reset your password: #{token}\n" <>
        "Email: #{user.email}\n" <>
        "Support: support@example.com\n"

    Mailer.deliver(user.email, subject, body)
  end

  def send_email_change_confirmation(user, token) do
    subject = "Confirm your new email"
    body =
      "Hello #{user.first_name},\n\n" <>
        "Use this token to confirm your email change: #{token}\n" <>
        "Email: #{user.email}\n" <>
        "Support: support@example.com\n"

    Mailer.deliver(user.email, subject, body)
  end
end

defmodule Mailer do
  def deliver(to, subject, body), do: {:ok, %{to: to, subject: subject, body: body}}
end
```
