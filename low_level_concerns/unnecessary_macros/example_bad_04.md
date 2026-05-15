smell_id:unnecessary_macros

# Example 04

This module uses a macro only to compare two values. It introduces metaprogramming where a normal function would be clearer.

```elixir
defmodule Rules do
  defmacro greater_than?(left, right) do
    quote do
      unquote(left) > unquote(right)
    end
  end
end

# Use examples
require Rules

Rules.greater_than?(price, 100)
Rules.greater_than?(10 + 2, 3)
```
