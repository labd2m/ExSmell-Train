smell_id:inappropriate_intimacy

# Example 07 - Job worker peeking into user notification preferences

```elixir
defmodule User do
  defstruct [:email, :phone, :notification_settings]
end

defmodule ReminderWorker do
  def deliver(%User{} = user, message) do
    email_enabled = user.notification_settings[:email]
    sms_enabled = user.notification_settings[:sms]

    cond do
      email_enabled and user.email ->
        {:email, user.email, message}

      sms_enabled and user.phone ->
        {:sms, user.phone, message}

      true ->
        :skip
    end
  end
end
```

The worker knows the shape and meaning of `notification_settings` instead of using a dedicated API from `User`.
