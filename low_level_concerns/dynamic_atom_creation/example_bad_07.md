smell_id:dynamic_atom_creation

# Example 7

```elixir
defmodule MyApp.FeatureFlags do
  def enabled?(flags, flag_name_from_db) do
    atom_key = String.to_atom(flag_name_from_db)
    Map.get(flags, atom_key, false)
  end
end

# Example:
# flags = %{new_checkout: true}
# MyApp.FeatureFlags.enabled?(flags, "campaign_2026_checkout")
```

Even though the flags map uses atoms, turning arbitrary database strings into atoms creates new atoms over time.
