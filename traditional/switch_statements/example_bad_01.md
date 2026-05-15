smell_id:switch_statements

# Example 01

```elixir
defmodule Billing do
  def charge_fee(total, %{plan: :free}), do: total
  def charge_fee(total, %{plan: :pro}), do: total + 5
  def charge_fee(total, %{plan: :enterprise}), do: total + 20
end

defmodule Shipping do
  def calculate_shipping(order_total, %{plan: :free}) do
    if order_total > 100 do
      0
    else
      15
    end
  end

  def calculate_shipping(_order_total, %{plan: :pro}), do: 0
  def calculate_shipping(_order_total, %{plan: :enterprise}), do: 0
end

defmodule Discounts do
  def apply_discount(total, %{plan: :free}) do
    if total > 200 do
      total * 0.95
    else
      total
    end
  end

  def apply_discount(total, %{plan: :pro}), do: total * 0.90
  def apply_discount(total, %{plan: :enterprise}), do: total * 0.85
end
```

The same `plan`-based conditional logic is spread across multiple modules. Adding a new `:team` plan would require updating several different switch-like branches.
