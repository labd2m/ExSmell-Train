smell_id:using_exceptions_for_control_flow

# Example 08

A CSV parser uses pattern matching that crashes for malformed rows, and the importer rescues to count invalid records.

```elixir
defmodule CsvRow do
  def parse_user!(row) do
    [name, email, age] = String.split(row, ",")
    %{name: name, email: email, age: String.to_integer(age)}
  end
end

defmodule UserImport do
  def import_row(row) do
    try do
      {:ok, CsvRow.parse_user!(row)}
    rescue
      _error ->
        {:invalid_row, row}
    end
  end
end

# Use examples
UserImport.import_row("Ana,ana@example.com,31")
UserImport.import_row("broken,row")
```
