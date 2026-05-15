smell_id:untested_polymorphic_behaviors

# Example 07

```elixir
defmodule Jsonish do
  @doc """
  Converts any enumerable into a map-like structure for later encoding.
  The API is overly generic and accepts values with odd semantics.
  """
  def normalize_pairs(data) do
    Enum.into(data, %{})
  end
end

# ...Use examples...

iex> Jsonish.normalize_pairs([{:name, "Lucas"}, {:role, "admin"}])
%{name: "Lucas", role: "admin"}

iex> Jsonish.normalize_pairs(%{a: 1, b: 2})
%{a: 1, b: 2}

iex> Jsonish.normalize_pairs(["name", "role"])
** (ArgumentError) argument error

iex> Jsonish.normalize_pairs(10..12)
** (ArgumentError) argument error
```
