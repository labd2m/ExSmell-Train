smell_id:complex_extractions_in_clauses

# Example 07 - Session access decision with nested data unpacked in the head

```elixir
defmodule Web.Session do
  defstruct [:user_id, :ip, :device, :expires_at, :scopes]
end

defmodule Web.Auth do
  alias Web.Session

  def access(
        action,
        %Session{user_id: user_id, ip: ip, device: device, expires_at: expires_at, scopes: scopes}
      )
      when action in [:read, :list] and is_list(scopes) and scopes != [] do
    {:ok, "user #{user_id} from #{ip} on #{device} can #{action}; expires_at=#{inspect(expires_at)}"}
  end

  def access(
        action,
        %Session{user_id: user_id, ip: ip, device: device, expires_at: expires_at, scopes: scopes}
      )
      when action in [:write, :delete] and is_list(scopes) and :admin in scopes do
    {:ok, "admin user #{user_id} from #{ip} on #{device} can #{action}; expires_at=#{inspect(expires_at)}"}
  end

  def access(
        action,
        %Session{user_id: user_id, ip: ip, device: device, expires_at: expires_at, scopes: scopes}
      )
      when is_list(scopes) do
    {:error, "user #{user_id} from #{ip} on #{device} cannot #{action}; scopes=#{inspect(scopes)}; expires_at=#{inspect(expires_at)}"}
  end
end
```

The smell lies in extracting many fields directly in each clause, even though only `scopes` is central to the branching and the rest serves the body.
