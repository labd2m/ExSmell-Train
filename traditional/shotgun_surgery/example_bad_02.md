smell_id:shotgun_surgery

# Shotgun Surgery - Example 02

```elixir
defmodule NotificationTitle do
  def title(:signup), do: "Welcome"
  def title(:password_reset), do: "Reset your password"
  # def title(:trial_expiring), do: "Your trial is ending"
end

defmodule NotificationTemplate do
  def template(:signup), do: "signup.html"
  def template(:password_reset), do: "password_reset.html"
  # def template(:trial_expiring), do: "trial_expiring.html"
end

defmodule NotificationChannels do
  def channels(:signup), do: [:email]
  def channels(:password_reset), do: [:email, :sms]
  # def channels(:trial_expiring), do: [:email, :push]
end

defmodule Notifications do
  def deliver(type, user) do
    %{
      title: NotificationTitle.title(type),
      template: NotificationTemplate.template(type),
      channels: NotificationChannels.channels(type),
      user: user
    }
  end
end
```
