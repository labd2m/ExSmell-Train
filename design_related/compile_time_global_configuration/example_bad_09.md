smell_id:compile_time_global_configuration

# Example 09 — Library stores token TTL in a module attribute

```elixir
# config/config.exs
import Config

config :token_tools,
  ttl_seconds: 3600

# lib/token_tools.ex
defmodule TokenTools do
  @ttl Application.fetch_env!(:token_tools, :ttl_seconds)

  def expires_at(now_unix) when is_integer(now_unix) do
    now_unix + @ttl
  end

  def expired?(issued_at, now_unix) when is_integer(issued_at) and is_integer(now_unix) do
    now_unix >= issued_at + @ttl
  end
end
```

## Use examples

```elixir
iex> TokenTools.expires_at(1_700_000_000)
1_700_003_600
```

## Why this is smelly

A library should not couple reusable behavior to compile-time application state through
module attributes. This can make tests brittle and configuration updates surprisingly inert.
