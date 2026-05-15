smell_id:unnecessary_macros

# Example 01

This example uses a macro just to add two values. A normal function would be simpler and easier to understand.

```elixir
defmodule Billing.Math do
  defmacro add(left, right) do
    quote do
      unquote(left) + unquote(right)
    end
  end
end

# Use examples
require Billing.Math

subtotal = 120
shipping = 15
Billing.Math.add(subtotal, shipping)
Billing.Math.add(100 + 20, 10 + 5)
```
