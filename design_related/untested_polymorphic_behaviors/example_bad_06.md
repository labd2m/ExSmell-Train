smell_id:untested_polymorphic_behaviors

# Example 06

```elixir
defmodule ListFormatter do
  @doc """
  Joins items with commas.
  It assumes the input is Enumerable, but documents no such requirement
  and accepts many values that do not really belong in a display list.
  """
  def comma_separated(items) do
    Enum.join(items, ",")
  end
end

# ...Use examples...

iex> ListFormatter.comma_separated(["a", "b", "c"])
"a,b,c"

iex> ListFormatter.comma_separated(1..3)
"1,2,3"

iex> ListFormatter.comma_separated(%{a: 1, b: 2})
"{:a, 1},{:b, 2}"

iex> ListFormatter.comma_separated(123)
** (Protocol.UndefinedError) protocol Enumerable not implemented for 123 of type Integer
```
