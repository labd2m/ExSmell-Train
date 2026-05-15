smell_id:speculative_assumptions

# Example 04 - File extension extraction from unexpected paths

```elixir
defmodule UploadPath do
  @doc """
  Returns the file extension for an upload path.

  This implementation assumes the last path segment contains a single dot.
  """
  def extension(path) do
    path
    |> String.split("/")
    |> List.last()
    |> String.split(".")
    |> Enum.at(1)
  end
end

# Planned input
UploadPath.extension("/tmp/report.pdf")
# => "pdf"

# Unplanned input with multiple dots extracts the wrong extension
UploadPath.extension("/tmp/archive.backup.tar.gz")
# => "backup"
```
