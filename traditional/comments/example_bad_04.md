smell_id:comments

# Example 04

```elixir
defmodule Analytics.ReportWindow do
  # Returns the date range used by the monthly report.
  # The first element of the tuple is the beginning of the month.
  # The second element is the end of the month.
  def month_bounds(%Date{} = date) do
    first_day = Date.beginning_of_month(date)
    last_day = Date.end_of_month(date)
    {first_day, last_day}
  end
end
```
