smell_id:duplicated_code

# Example 02

```elixir
defmodule Billing.Invoice do
  def subtotal(items) do
    items
    |> Enum.filter(&(&1.status == :active))
    |> Enum.map(&(&1.price * &1.quantity))
    |> Enum.sum()
  end

  def taxable_amount(items) do
    items
    |> Enum.filter(&(&1.status == :active))
    |> Enum.map(&(&1.price * &1.quantity))
    |> Enum.sum()
  end

  def total(items) do
    items
    |> Enum.filter(&(&1.status == :active))
    |> Enum.map(&(&1.price * &1.quantity))
    |> Enum.sum()
    |> Kernel.*(1.08)
  end
end
```
