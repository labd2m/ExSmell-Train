smell_id:agent_obsession

# Example 05 — Cache process read and mutated from many parts of the app

```elixir
defmodule Cache.Store do
  def start_link do
    Agent.start_link(fn -> %{} end, name: __MODULE__)
  end
end

defmodule Cache.Loader do
  def put_user(user_id, user) do
    Agent.update(Cache.Store, fn state ->
      Map.put(state, {:user, user_id}, user)
    end)
  end
end

defmodule Cache.Products do
  def put_product(product_id, product) do
    Agent.update(Cache.Store, fn state ->
      Map.put(state, {:product, product_id}, product)
    end)
  end
end

defmodule Cache.Invalidator do
  def invalidate(key) do
    Agent.update(Cache.Store, fn state ->
      Map.delete(state, key)
    end)
  end

  def clear_all do
    Agent.update(Cache.Store, fn _state -> %{} end)
  end
end

defmodule Cache.Reader do
  def fetch(key) do
    Agent.get(Cache.Store, fn state -> Map.get(state, key) end)
  end
end
```

The shared cache `Agent` is a global process that everybody talks to directly. That spreads knowledge of internal keys and makes the cache protocol impossible to centralize.
