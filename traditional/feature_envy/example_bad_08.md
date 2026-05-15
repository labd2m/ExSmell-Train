smell_id:feature_envy

# Example 08

```elixir
defmodule PayrollEntry do
  def load(employee_id) do
    %{
      employee_id: employee_id,
      hours: 160,
      hourly_rate: 40.0,
      overtime_hours: 12,
      overtime_rate: 60.0
    }
  end

  def gross(entry) do
    entry.hours * entry.hourly_rate
  end
end

defmodule PayrollReport do
  def employee_total(employee_id) do
    entry = PayrollEntry.load(employee_id)
    regular = PayrollEntry.gross(entry)
    overtime = entry.overtime_hours * entry.overtime_rate
    taxes = (regular + overtime) * 0.17

    regular + overtime - taxes
  end
end
```
