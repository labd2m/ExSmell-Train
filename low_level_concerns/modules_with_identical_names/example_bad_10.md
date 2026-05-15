smell_id:modules_with_identical_names

# Example 10

Reloading code in development hides that two files declare the same module with incompatible responsibilities.

## lib/app/search.ex
```elixir
defmodule App.Search do
  def run(query) do
    {:db, query}
  end
end
```

## lib/app/search_live.ex
```elixir
defmodule App.Search do
  def run(query) do
    {:live_cache, query}
  end

  def subscribe do
    :ok
  end
end
```

## Caller
```elixir
defmodule App.Controller do
  def index(params) do
    App.Search.run(params["q"])
  end
end
```

## Why this smells
The two files model different concepts but share the same module name. In a reloading environment this can appear to work, while one definition is actually replacing the other in memory.
