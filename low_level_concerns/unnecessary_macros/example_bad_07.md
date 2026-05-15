smell_id:unnecessary_macros

# Example 07

The macro below only checks whether a list is empty. This could be a normal function using `Enum.empty?/1` or pattern matching.

```elixir
defmodule ListChecks do
  defmacro empty?(list) do
    quote do
      length(unquote(list)) == 0
    end
  end
end

# Use examples
require ListChecks

ListChecks.empty?([])
ListChecks.empty?(items)
```
