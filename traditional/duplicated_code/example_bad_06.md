smell_id:duplicated_code

# Example 06

```elixir
defmodule Reports.Sales do
  def daily(rows) do
    rows
    |> Enum.filter(&(&1.state == :paid))
    |> Enum.group_by(& &1.salesperson_id)
    |> Enum.map(fn {salesperson_id, entries} ->
      total = Enum.reduce(entries, 0, fn row, acc -> acc + row.total end)
      %{salesperson_id: salesperson_id, total: total}
    end)
  end

  def weekly(rows) do
    rows
    |> Enum.filter(&(&1.state == :paid))
    |> Enum.group_by(& &1.salesperson_id)
    |> Enum.map(fn {salesperson_id, entries} ->
      total = Enum.reduce(entries, 0, fn row, acc -> acc + row.total end)
      %{salesperson_id: salesperson_id, total: total}
    end)
  end

  def monthly(rows) do
    rows
    |> Enum.filter(&(&1.state == :paid))
    |> Enum.group_by(& &1.salesperson_id)
    |> Enum.map(fn {salesperson_id, entries} ->
      total = Enum.reduce(entries, 0, fn row, acc -> acc + row.total end)
      %{salesperson_id: salesperson_id, total: total}
    end)
  end
end
```
