smell_id:shotgun_surgery

# Shotgun Surgery - Example 04

```elixir
defmodule PayrollBonus do
  def bonus(%{level: :junior}, salary), do: salary * 0.05
  def bonus(%{level: :senior}, salary), do: salary * 0.12
  # def bonus(%{level: :staff}, salary), do: salary * 0.18
end

defmodule VacationPolicy do
  def days(%{level: :junior}), do: 15
  def days(%{level: :senior}), do: 25
  # def days(%{level: :staff}), do: 30
end

defmodule EquipmentPolicy do
  def budget(%{level: :junior}), do: 800
  def budget(%{level: :senior}), do: 2_000
  # def budget(%{level: :staff}), do: 3_500
end
```
