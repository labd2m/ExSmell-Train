smell_id:primitive_obsession

# Primitive Obsession - Example 06

```elixir
defmodule Payroll do
  @moduledoc false

  # salary, bonus, and discount are floats; role and contract are strings.
  def monthly_pay(employee_id, salary, bonus, discount, role, contract_type) do
    net = salary + bonus - discount

    %{
      employee_id: employee_id,
      role: role,
      contract_type: contract_type,
      net: Float.round(net, 2)
    }
  end
end

# ...Use examples...

Payroll.monthly_pay("e-9", 4000.00, 500.00, 120.35, "developer", "clt")
Payroll.monthly_pay("e-9", 4000, "500", nil, :developer, :contract)
```
