smell_id:unnecessary_macros

# Example 03

This macro creates an `{:ok, value}` tuple. A plain function would be much more direct.

```elixir
defmodule ResponseHelpers do
  defmacro ok(value) do
    quote do
      {:ok, unquote(value)}
    end
  end
end

# Use examples
require ResponseHelpers

ResponseHelpers.ok(user)
ResponseHelpers.ok(%{id: 1, name: "Ada"})
```
