smell_id:comments

# Example 07

```elixir
defmodule Importers.CsvUsers do
  # Parses CSV rows into a user map.
  # The first row is considered the header.
  # Empty trailing rows are ignored.
  def parse(rows) do
    [header | data_rows] = rows

    data_rows
    |> Enum.reject(&(&1 == []))
    |> Enum.map(fn row ->
      header
      |> Enum.zip(row)
      |> Enum.into(%{})
    end)
  end
end
```
