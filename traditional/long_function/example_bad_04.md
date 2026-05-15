smell_id:long_function

```elixir
defmodule Payroll do
  def process(employee, hours, bonus, tax_rate, repo) do
    IO.puts("Processing payroll for #{employee.name}")

    regular_hours = min(hours, 40)
    overtime_hours = max(hours - 40, 0)

    regular_pay = regular_hours * employee.hourly_rate
    overtime_pay = overtime_hours * employee.hourly_rate * 1.5
    gross = regular_pay + overtime_pay + bonus

    insurance = if employee.full_time, do: 125, else: 0
    retirement = if employee.opt_in_retirement, do: gross * 0.05, else: 0
    taxes = gross * tax_rate
    net = gross - insurance - retirement - taxes

    summary = %{
      employee_id: employee.id,
      regular_hours: regular_hours,
      overtime_hours: overtime_hours,
      regular_pay: regular_pay,
      overtime_pay: overtime_pay,
      bonus: bonus,
      insurance: insurance,
      retirement: retirement,
      taxes: taxes,
      net: net
    }

    case repo.insert(summary) do
      {:ok, _record} ->
        IO.puts("Payroll persisted for #{employee.name}")
        IO.puts("Net pay: #{net}")
        summary

      {:error, reason} ->
        IO.puts("Could not save payroll: #{inspect(reason)}")
        raise "payroll failure"
    end
  end
end
```
