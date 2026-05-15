smell_id:switch_statements

# Example 06

```elixir
defmodule FileImporter do
  def parse(content, :csv), do: CSV.decode!(content) |> Enum.to_list()
  def parse(content, :json), do: Jason.decode!(content)
  def parse(content, :xml), do: SweetXml.parse(content)
end

defmodule FileValidator do
  def valid_extension?(".csv", :csv), do: true
  def valid_extension?(".json", :json), do: true
  def valid_extension?(".xml", :xml), do: true
  def valid_extension?(_, _), do: false
end

defmodule FileLogger do
  def event_name(:csv), do: "csv_import"
  def event_name(:json), do: "json_import"
  def event_name(:xml), do: "xml_import"
end
```

The format-specific conditionals are duplicated across parser, validator, and logger code. Adding YAML support would require several separate edits.
