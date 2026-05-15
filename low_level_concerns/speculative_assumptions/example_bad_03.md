smell_id:speculative_assumptions

# Example 03 - Authorization header parsing that never fails loudly

```elixir
defmodule AuthHeader do
  @doc """
  Extracts the bearer token from an Authorization header.

  The function assumes the header is always formatted as `Bearer TOKEN`.
  """
  def token(header) do
    header
    |> String.split(" ")
    |> Enum.at(1)
  end
end

# Planned input
AuthHeader.token("Bearer abc123")
# => "abc123"

# Unplanned input returns the wrong thing instead of crashing
AuthHeader.token("Token type=Bearer abc123")
# => "type=Bearer"
```
