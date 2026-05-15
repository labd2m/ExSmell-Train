smell_id:using_exceptions_for_control_flow

# Example 02

A token decoder raises when a token is missing or malformed, and the plug treats those ordinary outcomes by rescuing.

```elixir
defmodule AuthToken do
  def subject!("user:" <> id) when byte_size(id) > 0 do
    id
  end

  def subject!(_token) do
    raise RuntimeError, "invalid token"
  end
end

defmodule ApiAuthPlug do
  def authenticate(headers) do
    try do
      user_id = AuthToken.subject!(headers["authorization"])
      {:ok, %{current_user_id: user_id}}
    rescue
      e in RuntimeError ->
        {:error, %{status: 401, reason: e.message}}
    end
  end
end

# Use examples
ApiAuthPlug.authenticate(%{"authorization" => "user:42"})
ApiAuthPlug.authenticate(%{"authorization" => "guest"})
```
