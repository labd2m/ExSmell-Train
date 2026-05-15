smell_id:long_function

```elixir
defmodule NotificationDispatcher do
  def deliver(event, users, email_client, sms_client) do
    active_users = Enum.filter(users, fn user -> user.active end)

    message =
      case event.type do
        :incident -> "Incident detected: #{event.title}"
        :maintenance -> "Scheduled maintenance: #{event.title}"
        :marketing -> "Special campaign: #{event.title}"
      end

    Enum.each(active_users, fn user ->
      channels = user.channels || []

      if :email in channels do
        payload = %{to: user.email, subject: event.title, body: message}
        email_client.send(payload)
        IO.puts("Email sent to #{user.email}")
      end

      if :sms in channels do
        sms_client.send(user.phone, message)
        IO.puts("SMS sent to #{user.phone}")
      end

      if :push in channels do
        IO.puts("Push notification queued for #{user.id}")
      end
    end)

    %{sent_to: length(active_users), event: event.type}
  end
end
```
