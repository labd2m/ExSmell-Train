smell_id:alternative_return_types

# Example 09 - Query executor returning a list or a stream

```elixir
defmodule MyApp.Reporting do
  alias MyApp.Repo

  def run(query, opts \ []) do
    case opts[:stream] do
      true ->
        Repo.stream(query)

      _ ->
        Repo.all(query)
    end
  end
end

# Usage
MyApp.Reporting.run(from u in "users")
# => [%{id: 1}, %{id: 2}]

MyApp.Reporting.run(from u in "users", stream: true)
# => #Stream<...>
```

Consumers now need to handle either an eagerly-loaded list or a lazy stream depending on a flag.
