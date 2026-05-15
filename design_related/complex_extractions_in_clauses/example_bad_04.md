smell_id:complex_extractions_in_clauses

# Example 04 - Notification routing with many extracted fields

```elixir
defmodule Messaging.Notification do
  defstruct [:user_id, :email, :phone, :channel, :enabled, :locale]
end

defmodule Messaging.Router do
  alias Messaging.Notification

  def route(%Notification{
        user_id: user_id,
        email: email,
        phone: phone,
        channel: channel,
        enabled: enabled,
        locale: locale
      })
      when enabled == true and channel == :email and locale in ["en", "pt-BR"] do
    {:send_email, "notify #{user_id} using #{email} (#{locale}) via #{channel}"}
  end

  def route(%Notification{
        user_id: user_id,
        email: email,
        phone: phone,
        channel: channel,
        enabled: enabled,
        locale: locale
      })
      when enabled == true and channel == :sms and locale in ["en", "pt-BR"] do
    {:send_sms, "notify #{user_id} using #{phone} (#{locale}) via #{channel}; backup=#{email}"}
  end

  def route(%Notification{
        user_id: user_id,
        email: email,
        phone: phone,
        channel: channel,
        enabled: enabled,
        locale: locale
      })
      when enabled == false do
    {:skip, "notification disabled for #{user_id}; channel=#{channel}; email=#{email}; phone=#{phone}; locale=#{locale}"}
  end
end
```

Each clause extracts all fields up front, making it harder to understand which ones drive the branching and which ones are just used in the body text.
