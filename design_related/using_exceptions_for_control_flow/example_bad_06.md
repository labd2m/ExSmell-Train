smell_id:using_exceptions_for_control_flow

# Example 06

The cache API raises on a normal cache miss, forcing callers to rescue instead of handling a tagged return.

```elixir
defmodule CacheLayer do
  def get!(store, key) do
    case Map.fetch(store, key) do
      {:ok, value} -> value
      :error -> raise RuntimeError, "cache miss"
    end
  end
end

defmodule ProductPage do
  def load(store, product_id) do
    cache_key = "product:" <> product_id

    try do
      {:from_cache, CacheLayer.get!(store, cache_key)}
    rescue
      e in RuntimeError ->
        {:recompute, %{key: cache_key, reason: e.message}}
    end
  end
end

# Use examples
ProductPage.load(%{"product:1" => %{name: "Mouse"}}, "1")
ProductPage.load(%{}, "2")
```
