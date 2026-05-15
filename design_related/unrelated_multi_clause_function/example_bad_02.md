smell_id:unrelated_multi_clause_function

# Example 02

This example places **CSV import**, **API token rotation**, and **image thumbnail generation** under one function name: `run/1`.

```elixir
defmodule MyApp.Operations do
  defmodule CsvImport do
    defstruct [:path, :separator]
  end

  defmodule TokenRotation do
    defstruct [:service, :expires_at]
  end

  defmodule ThumbnailJob do
    defstruct [:image_path, :size]
  end

  @doc """
  Run an operation.
  """
  def run(%CsvImport{path: path, separator: sep}) when is_binary(path) and sep in [",", ";"] do
    File.stream!(path)
    |> Enum.map(&String.split(&1, sep))
  end

  # unrelated: credentials management
  def run(%TokenRotation{service: service, expires_at: expires_at}) when is_binary(service) do
    %{service: service, rotated: true, previous_expiration: expires_at}
  end

  # unrelated: media processing
  def run(%ThumbnailJob{image_path: path, size: {w, h}}) when w > 0 and h > 0 do
    "thumbnail generated for #{path} at #{w}x#{h}"
  end
end
```
