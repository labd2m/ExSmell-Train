smell_id:long_parameter_list

```elixir
defmodule AuthTokens do
  def issue(user_id, email, role, device_id, ip_address, user_agent, ttl_seconds, refreshable) do
    %{
      sub: user_id,
      email: email,
      role: role,
      device_id: device_id,
      ip_address: ip_address,
      user_agent: user_agent,
      ttl_seconds: ttl_seconds,
      refreshable: refreshable
    }
  end
end

AuthTokens.issue(
  123,
  "person@example.com",
  :member,
  "ios-001",
  "10.0.0.5",
  "Safari",
  3600,
  true
)
```
