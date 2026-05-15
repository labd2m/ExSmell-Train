smell_id:speculative_assumptions

# Example 02 - CSV column extraction that guesses too much

```elixir
defmodule CsvRow do
  @doc """
  Extracts the e-mail column from a CSV row.

  The code assumes the row always has exactly three comma-separated values.
  """
  def email(row) do
    row
    |> String.split(",")
    |> Enum.at(2)
  end
end

# Planned input
CsvRow.email("42,Ana,ana@example.com")
# => "ana@example.com"

# Unplanned input with an extra comma in the name field
CsvRow.email("42,Ana,Silva,ana@example.com")
# => "Silva"
```
