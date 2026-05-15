smell_id:switch_statements

# Example 09

```elixir
defmodule ReportExporter do
  def extension(:pdf), do: ".pdf"
  def extension(:csv), do: ".csv"
  def extension(:xlsx), do: ".xlsx"
end

defmodule ReportMime do
  def mime(:pdf), do: "application/pdf"
  def mime(:csv), do: "text/csv"
  def mime(:xlsx), do: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
end

defmodule ReportStoragePath do
  def folder(:pdf), do: "reports/pdf"
  def folder(:csv), do: "reports/csv"
  def folder(:xlsx), do: "reports/xlsx"
end


defmodule Reports do
  def target_path(name, format) do
    Path.join(ReportStoragePath.folder(format), name <> ReportExporter.extension(format))
  end
end
```

Export format decisions are repeated across several modules. Supporting a new format means editing multiple switch-like condition definitions.
