smell_id:compile_time_global_configuration

# Example 06 — URL builder with base host compiled from config

```elixir
# config/runtime.exs or config/config.exs
import Config

config :url_lib,
  base_host: "https://api.example.com"

# lib/url_lib/builder.ex
defmodule UrlLib.Builder do
  @host Application.fetch_env!(:url_lib, :base_host)

  def user_url(id) when is_integer(id) do
    @host <> "/users/" <> Integer.to_string(id)
  end

  def order_url(id) when is_integer(id) do
    @host <> "/orders/" <> Integer.to_string(id)
  end
end
```

## Use examples

```elixir
iex> UrlLib.Builder.user_url(10)
"https://api.example.com/users/10"
```

## Why this is smelly

The module attribute `@host` is computed at compile-time from global config. If the host
is intended to be environment-dependent or change between deployments, the compiled module
can become stale or fail to compile when config is unavailable.
