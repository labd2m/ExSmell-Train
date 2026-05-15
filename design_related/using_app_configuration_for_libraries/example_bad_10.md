id:uacl_10
smell_id:using_app_configuration_for_libraries

# Example 10 - Token signer using app env instead of explicit parameters

```elixir
# config/config.exs
import Config

config :token_tools,
  algorithm: :sha256,
  secret: "global-secret"
```

```elixir
defmodule TokenTools do
  def sign(payload) when is_binary(payload) do
    algorithm = Application.fetch_env!(:token_tools, :algorithm)
    secret = Application.fetch_env!(:token_tools, :secret)

    :crypto.mac(:hmac, algorithm, secret, payload)
    |> Base.encode16(case: :lower)
  end
end
```

```elixir
defmodule PublicApiToken do
  def sign(payload), do: TokenTools.sign(payload)
end

defmodule WebhookToken do
  def sign(payload), do: TokenTools.sign(payload)
end
```

```elixir
iex> PublicApiToken.sign("user:10")
"...signature..."

iex> WebhookToken.sign("user:10")
"...signature..."
```

Different consumers may need different secrets or algorithms, but the library
can only behave one way because it reads its configuration from the global
application environment.
