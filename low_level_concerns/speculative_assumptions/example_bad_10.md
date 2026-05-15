smell_id:speculative_assumptions

# Example 10 - Import job assumes a fixed line format and keeps going

```elixir
defmodule MyApp.LegacyLineParser do
  @doc """
  Extracts the status from a legacy line formatted as `id|name|status`.

  Instead of pattern matching on the expected format, the parser just picks the
  third element and accepts whatever is there.
  """
  def status(line) do
    line
    |> String.split("|")
    |> Enum.at(2)
  end
end

defmodule MyApp.ImportJob do
  def import_line(line) do
    status = MyApp.LegacyLineParser.status(line)
    {:ok, %{raw: line, status: status}}
  end
end

# Planned input
MyApp.ImportJob.import_line("10|Maria|active")
# => {:ok, %{raw: "10|Maria|active", status: "active"}}

# Unplanned input gives a plausible but incorrect value
MyApp.ImportJob.import_line("10|Maria|status|active")
# => {:ok, %{raw: "10|Maria|status|active", status: "status"}}
```
