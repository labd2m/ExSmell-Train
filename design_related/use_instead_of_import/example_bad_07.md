smell_id:use_instead_of_import

# Example 07 - Parser helpers mixed into a CSV module through `use`

This client really just needs parsing helpers. The `use` relationship adds indirection
and can introduce conflicts that are surprising at compile time.

```elixir
defmodule ParserHelpers do
  def parse(line), do: String.split(line, ",")
  def parse_int(value), do: String.to_integer(value)
end

defmodule CsvLibrary do
  defmacro __using__(_opts) do
    quote do
      import ParserHelpers

      def csv_lib?, do: true
    end
  end

  def csv_lib?, do: true
end

defmodule SalesCsv do
  use CsvLibrary

  def parse(_line) do
    {:custom_csv_parser}
  end

  def read_amount(line) do
    [_, amount] = parse(line)
    parse_int(amount)
  end
end
```

`SalesCsv` appears to depend only on `CsvLibrary`, but that `use` quietly imports
`ParserHelpers`. The local `parse/1` function conflicts with what the library injects.
