smell_id:unnecessary_macros

# Example 06

This code defines a logging macro even though it only calls `IO.puts/1`. Nothing about this behavior requires a macro.

```elixir
defmodule Audit do
  defmacro log(message) do
    quote do
      IO.puts("AUDIT: " <> to_string(unquote(message)))
    end
  end
end

# Use examples
require Audit

Audit.log("user signed in")
Audit.log(user.id)
```
