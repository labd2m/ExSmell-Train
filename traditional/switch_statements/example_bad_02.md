smell_id:switch_statements

# Example 02

```elixir
defmodule NotificationPreferences do
  def channel_for(%{contact_preference: :email}), do: :email
  def channel_for(%{contact_preference: :sms}), do: :sms
  def channel_for(%{contact_preference: :push}), do: :push
end

defmodule NotificationFormatter do
  def format_message(user, %{contact_preference: :email}) do
    "EMAIL TO #{user.email}: welcome"
  end

  def format_message(user, %{contact_preference: :sms}) do
    "SMS TO #{user.phone}: welcome"
  end

  def format_message(user, %{contact_preference: :push}) do
    "PUSH TO #{user.device_id}: welcome"
  end
end

defmodule NotificationAudit do
  def audit_kind(%{contact_preference: :email}), do: "email_notification"
  def audit_kind(%{contact_preference: :sms}), do: "sms_notification"
  def audit_kind(%{contact_preference: :push}), do: "push_notification"
end
```

The same branching by `contact_preference` is duplicated across three modules. A new preference such as `:whatsapp` would force multiple synchronized changes.
