smell_id:working_with_invalid_data
# Example 07 — Exporting malformed rows with `NimbleCSV`

This function assumes it received a list of rows and forwards the value directly to `NimbleCSV.RFC4180.dump_to_iodata/1`.

## Bad code

```elixir
defmodule MyApp.Exports do
  alias NimbleCSV.RFC4180, as: CSV

  def export_rows(rows) do
    # expected:
    #   rows -> list of lists, e.g. [["id", "name"], ["1", "Alice"]]
    #
    # bad smell:
    #   no shape validation before the CSV library
    rows
    |> CSV.dump_to_iodata()
    |> IO.iodata_to_binary()
  end
end
```

## Example usage

```elixir
iex> MyApp.Exports.export_rows([["id", "name"], ["1", "Alice"]])
"id,name\r\n1,Alice\r\n"

iex> MyApp.Exports.export_rows(%{id: 1, name: "Alice"})
# representative failure from inside the CSV implementation
** (Protocol.UndefinedError) protocol Enumerable not implemented for %{id: 1, name: "Alice"}
```

## Why this is a smell

The caller gets a low-level CSV/enumeration failure instead of a direct statement that `rows` had the wrong shape.
That makes the defect harder to localize to the export boundary.
