smell_id:working_with_invalid_data
# Example 01 — Using `Decimal` without validating inputs

This example shows a function that accepts any value for `discount` and forwards it directly to `Decimal.sub/2`.  
When invalid data crosses the boundary, the failure happens inside `Decimal`, not at the application boundary.

## Bad code

```elixir
defmodule MyApp.Checkout do
  alias Decimal, as: D

  def apply_discount(total, discount) do
    # expected:
    #   total    -> %Decimal{}
    #   discount -> %Decimal{}
    #
    # bad smell:
    #   no validation at the boundary
    D.sub(total, discount)
  end
end
```

## Example usage

```elixir
iex> total = Decimal.new("100.00")
iex> MyApp.Checkout.apply_discount(total, Decimal.new("10.00"))
#Decimal<90.00>

iex> total = Decimal.new("100.00")
iex> MyApp.Checkout.apply_discount(total, "10.00")
** (FunctionClauseError) no function clause matching in Decimal.sub/2
```

## Why this is a smell

`apply_discount/2` does not validate that both arguments are `Decimal` structs before calling the library.
The error surfaces deep inside the dependency, which makes the true source of the problem harder to spot.
