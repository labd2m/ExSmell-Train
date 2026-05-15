smell_id:untested_polymorphic_behaviors

# Example 10

```elixir
defmodule JoinPath do
  @doc """
  Joins a user-supplied path segment to a base path.
  The function depends on String.Chars and never states what kinds of values
  are valid path segments.
  """
  def under_tmp(segment) do
    Path.join("/tmp", to_string(segment))
  end
end

# ...Use examples...

iex> JoinPath.under_tmp("uploads")
"/tmp/uploads"

iex> JoinPath.under_tmp(15)
"/tmp/15"

iex> JoinPath.under_tmp(URI.parse("https://example.com/x"))
"/tmp/https://example.com/x"

iex> JoinPath.under_tmp(%{dir: "uploads"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{dir: "uploads"} of type Map
```
