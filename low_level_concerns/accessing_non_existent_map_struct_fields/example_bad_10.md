smell_id:accessing_non_existent_map_struct_fields

# Example 10 - Nested map traversal with dynamic access

```elixir
defmodule ThemeLoader do
  def button_color(settings) do
    settings[:theme][:button][:color]
  end
end

# Use examples
# iex> ThemeLoader.button_color(%{theme: %{button: %{color: "blue"}}})
# "blue"
#
# iex> ThemeLoader.button_color(%{theme: %{button: %{}}})
# nil
#
# iex> ThemeLoader.button_color(%{theme: %{button: %{color: nil}}})
# nil
```

This is especially risky because the code relies on several dynamic accesses in sequence. Missing nested keys and present keys with `nil` values are conflated, making debugging difficult.
