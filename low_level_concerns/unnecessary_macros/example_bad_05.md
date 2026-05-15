smell_id:unnecessary_macros

# Example 05

This macro just delegates to `Map.get/3`. It makes a simple runtime operation look like something special.

```elixir
defmodule Params do
  defmacro fetch(map, key, default \\ nil) do
    quote do
      Map.get(unquote(map), unquote(key), unquote(default))
    end
  end
end

# Use examples
require Params

params = %{"page" => "1"}
Params.fetch(params, "page")
Params.fetch(params, "sort", "name")
```
