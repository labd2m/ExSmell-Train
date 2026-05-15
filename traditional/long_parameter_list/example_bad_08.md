smell_id:long_parameter_list

```elixir
defmodule CsvImport do
  def import_row(row_id, file_name, account_id, source, delimiter, has_headers, retry_count, notify_email) do
    %{
      row_id: row_id,
      file_name: file_name,
      account_id: account_id,
      source: source,
      delimiter: delimiter,
      has_headers: has_headers,
      retry_count: retry_count,
      notify_email: notify_email
    }
  end
end

CsvImport.import_row(
  10,
  "users.csv",
  999,
  :s3,
  ",",
  true,
  3,
  "ops@example.com"
)
```
