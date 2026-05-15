smell_id:long_parameter_list

```elixir
defmodule Notifications do
  def send_email(to_name, to_email, subject, body, from_name, from_email, reply_to, track_opens, locale) do
    %{
      to: {to_name, to_email},
      from: {from_name, from_email},
      reply_to: reply_to,
      subject: subject,
      body: body,
      track_opens: track_opens,
      locale: locale
    }
  end
end

Notifications.send_email(
  "Maria",
  "maria@example.com",
  "Welcome",
  "Hello there",
  "Support",
  "support@example.com",
  "help@example.com",
  true,
  "en"
)
```
