smell_id:untested_polymorphic_behaviors

# Example 02

```elixir
defmodule CsvLine do
  @doc """
  Converts a value into a one-cell CSV line.
  The API looks generic, but only some protocol implementations work.
  """
  def encode_cell(value) do
    "\"#{to_string(value)}\"\n"
  end
end

# ...Use examples...

iex> CsvLine.encode_cell("Alice")
"\"Alice\"\n"

iex> CsvLine.encode_cell(99.5)
"\"99.5\"\n"

iex> CsvLine.encode_cell(~D[2026-04-22])
"\"2026-04-22\"\n"

iex> CsvLine.encode_cell(%{name: "Alice"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{name: "Alice"} of type Map
```
