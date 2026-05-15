id:uacl_02
smell_id:using_app_configuration_for_libraries

# Example 02 - CSV exporter controlled by global delimiter and header policy

```elixir
# config/config.exs
import Config

config :csv_tools,
  delimiter: ";",
  include_header: true
```

```elixir
defmodule CsvTools.Exporter do
  def export(rows) when is_list(rows) do
    delimiter = Application.fetch_env!(:csv_tools, :delimiter)
    include_header = Application.fetch_env!(:csv_tools, :include_header)

    [first | _] = rows
    columns = Map.keys(first)

    header =
      if include_header do
        Enum.join(columns, delimiter) <> "\n"
      else
        ""
      end

    body =
      rows
      |> Enum.map(fn row ->
        columns
        |> Enum.map(&Map.get(row, &1))
        |> Enum.join(delimiter)
      end)
      |> Enum.join("\n")

    header <> body
  end
end
```

```elixir
defmodule BillingReport do
  alias CsvTools.Exporter

  def render(rows), do: Exporter.export(rows)
end

defmodule PartnerFeed do
  alias CsvTools.Exporter

  def render(rows), do: Exporter.export(rows)
end
```

```elixir
iex> rows = [%{"id" => "1", "name" => "Amp"}]
iex> BillingReport.render(rows)
"id;name\n1;Amp"

iex> PartnerFeed.render(rows)
"id;name\n1;Amp"
```

The library cannot be reused with one caller needing comma-separated output and
another needing semicolon-separated output, because configuration is global.
