smell_id:alternative_return_types

# Example 02 - Token verification returning either a boolean or claims map

```elixir
defmodule MyApp.Token do
  def verify(token, opts \ []) do
    case opts[:return] do
      :claims ->
        {:ok, claims} = ThirdPartyJWT.verify_and_validate(token)
        claims

      _ ->
        case ThirdPartyJWT.verify_and_validate(token) do
          {:ok, _claims} -> true
          _ -> false
        end
    end
  end
end

# Usage
MyApp.Token.verify(token)
# => true

MyApp.Token.verify(token, return: :claims)
# => %{"sub" => "123", "role" => "admin"}
```

The same function may return a boolean or a map, depending on an optional flag.
