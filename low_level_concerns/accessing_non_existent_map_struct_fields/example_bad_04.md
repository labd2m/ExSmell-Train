smell_id:accessing_non_existent_map_struct_fields

# Example 04 - Feature flag configuration in a GenServer

```elixir
defmodule CacheServer do
  use GenServer

  def start_link(config) do
    GenServer.start_link(__MODULE__, config, name: __MODULE__)
  end

  @impl true
  def init(config) do
    state = %{
      ttl: config[:ttl] || 60,
      warmup: config[:warmup] || false
    }

    {:ok, state}
  end
end

# Use examples
# CacheServer.start_link(%{ttl: 120})
# CacheServer.start_link(%{})
# CacheServer.start_link(%{ttl: nil})
```

`config[:ttl]` returning `nil` when the key is absent or explicitly nil means invalid configuration is accepted and normalized without clarity.
