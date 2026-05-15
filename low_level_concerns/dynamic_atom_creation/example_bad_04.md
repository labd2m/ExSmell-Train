smell_id:dynamic_atom_creation

# Example 4

```elixir
defmodule MyApp.CsvImporter do
  def import_row(row) do
    row
    |> Enum.map(fn {column, value} ->
      {String.to_atom(column), value}
    end)
    |> Enum.into(%{})
  end
end

# Example row coming from a CSV file:
# %{"email" => "ana@example.com", "age" => "30", "custom_field_98123" => "x"}
```

CSV headers are external and unbounded. Converting all of them with `String.to_atom/1` is dangerous.
