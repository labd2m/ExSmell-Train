smell_id:dynamic_atom_creation

# Example 6

```elixir
defmodule MyApp.SocketAuth do
  def build_claims(headers) do
    Enum.reduce(headers, %{}, fn {key, value}, acc ->
      Map.put(acc, String.to_atom(key), value)
    end)
  end
end

# Example headers:
# [{"x-user-id", "10"}, {"x-role", "admin"}, {"x-tenant-12345", "acme"}]
```

HTTP header names can vary a lot and may be fully controlled by clients.
