smell_id:inappropriate_intimacy

# Example 05 - Controller decoding token internals itself

```elixir
defmodule AuthToken do
  defstruct [:raw, :claims]
end

defmodule SessionController do
  def current_user_id(%AuthToken{} = token) do
    issued_at = token.claims["iat"]
    user_id = token.claims["sub"]
    role = token.claims["role"]

    if role == "user" and issued_at > 0 do
      user_id
    else
      nil
    end
  end
end
```

Instead of asking `AuthToken` for authenticated information, the controller digs through token internals and embeds its own auth rules.
