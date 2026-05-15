smell_id:compile_time_global_configuration

# Example 07 — Cache namespace captured before the app env is ready

```elixir
# config/config.exs
import Config

config :cache_helpers,
  namespace: "prod-cache"

# lib/cache_helpers/key_builder.ex
defmodule CacheHelpers.KeyBuilder do
  @namespace Application.fetch_env!(:cache_helpers, :namespace)

  def build(parts) when is_list(parts) do
    Enum.join([@namespace | parts], ":")
  end
end
```

## Use examples

```elixir
iex> CacheHelpers.KeyBuilder.build(["users", "42", "profile"])
"prod-cache:users:42:profile"
```

## Possible compilation issue

```text
warning: Application.fetch_env!/2 is discouraged in the module body,
use Application.compile_env/3 instead
```

## Why this is smelly

The module reads from the Application Environment while compiling the library. That makes
the code sensitive to compilation order and reduces flexibility.
