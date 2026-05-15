smell_id:modules_with_identical_names

# Example 05

Environment-specific files define the same module differently.

## config/runtime/dev_cache.ex
```elixir
defmodule Cache.Client do
  def put(key, value) do
    Process.put({:cache, key}, value)
    :ok
  end
end
```

## config/runtime/prod_cache.ex
```elixir
defmodule Cache.Client do
  def put(key, value) do
    Redix.command!(:redix, ["SET", key, value])
  end

  def backend do
    :redis
  end
end
```

## Caller
```elixir
defmodule Cache.Warmer do
  def warm(entries) do
    Enum.each(entries, fn {key, value} ->
      Cache.Client.put(key, value)
    end)
  end
end
```

## Why this smells
This is not a behavior plus implementation swap. It is the same module name being defined in multiple places with different APIs, which leads to redefinition and unstable semantics.
