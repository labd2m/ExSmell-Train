smell_id:using_exceptions_for_control_flow

# Example 09

A feature flag helper raises when a flag is absent, and the caller rescues to choose the normal disabled path.

```elixir
defmodule Flags do
  def enabled!(flags, key) do
    case Map.fetch(flags, key) do
      {:ok, true} -> true
      {:ok, false} -> false
      :error -> raise RuntimeError, "flag #{inspect(key)} not configured"
    end
  end
end

defmodule LandingPage do
  def hero_variant(flags) do
    try do
      if Flags.enabled!(flags, :new_hero) do
        :new_layout
      else
        :old_layout
      end
    rescue
      _ ->
        :old_layout
    end
  end
end

# Use examples
LandingPage.hero_variant(%{new_hero: true})
LandingPage.hero_variant(%{})
```
