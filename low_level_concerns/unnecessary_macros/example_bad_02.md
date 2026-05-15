smell_id:unnecessary_macros

# Example 02

This macro only wraps `String.trim/1`. There is no compile-time code generation benefit here.

```elixir
defmodule TextTools do
  defmacro clean(value) do
    quote do
      String.trim(unquote(value))
    end
  end
end

# Use examples
require TextTools

TextTools.clean("  hello  ")
TextTools.clean(user_input)
```
