smell_id:dynamic_atom_creation

# Example 8

```elixir
defmodule MyApp.ConfigLoader do
  def load(entries) do
    Enum.into(entries, %{}, fn {name, value} ->
      {String.to_atom(name), value}
    end)
  end
end

# Example entries:
# [{"pool_size", "10"}, {"timeout", "5000"}, {"customer_841_mode", "strict"}]
```

This loader converts all configuration names into atoms, including names that may be tenant-specific or user-defined.
