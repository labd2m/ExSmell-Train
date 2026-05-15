smell_id:use_instead_of_import

# Example 10 - Date helpers hidden by a reporting library

This example shows a reporting module that only needs helper functions. The dependency
should be explicit with `import`, but `use` injects code and propagates names instead.

```elixir
defmodule DateHelpers do
  def today, do: Date.utc_today()
  def iso(date), do: Date.to_iso8601(date)
end

defmodule ReportingLibrary do
  defmacro __using__(_opts) do
    quote do
      import DateHelpers

      def reporting_enabled?, do: true
    end
  end

  def reporting_enabled?, do: true
end

defmodule DailyReport do
  use ReportingLibrary

  def today do
    ~D[1999-12-31]
  end

  def filename do
    "report-" <> iso(today()) <> ".csv"
  end
end
```

A reader must inspect `ReportingLibrary` to discover that `DateHelpers` is imported.
That broader `use` relationship hides the dependency and conflicts with the local
`today/0` function.
