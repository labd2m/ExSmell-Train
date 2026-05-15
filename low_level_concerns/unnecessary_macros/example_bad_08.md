smell_id:unnecessary_macros

# Example 08

This macro calculates a discounted price. It performs ordinary arithmetic and does not need compile-time expansion.

```elixir
defmodule Pricing do
  defmacro apply_discount(amount, percent) do
    quote do
      unquote(amount) - unquote(amount) * unquote(percent) / 100
    end
  end
end

# Use examples
require Pricing

Pricing.apply_discount(200, 10)
Pricing.apply_discount(total, customer_discount)
```
