smell_id:untested_polymorphic_behaviors

# Example 04

```elixir
defmodule SearchQuery do
  @doc """
  Creates a query parameter string.
  The function is protocol-dependent but never constrains what a "term" should be.
  """
  def build(term) do
    "q=" <> URI.encode(to_string(term))
  end
end

# ...Use examples...

iex> SearchQuery.build("phoenix framework")
"q=phoenix%20framework"

iex> SearchQuery.build(42)
"q=42"

iex> SearchQuery.build(URI.parse("https://example.com/docs"))
"q=https%3A%2F%2Fexample.com%2Fdocs"

iex> SearchQuery.build(%{tag: "elixir"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{tag: "elixir"} of type Map
```
