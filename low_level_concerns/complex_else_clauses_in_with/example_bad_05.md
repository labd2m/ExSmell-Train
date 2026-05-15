smell_id:complex_else_clauses_in_with

# Example 05

```elixir
defmodule MyApp.ImagePipeline do
  def process_upload(path) do
    with {:ok, binary} <- File.read(path),
         {:ok, image} <- MyApp.Image.decode(binary),
         {:ok, resized} <- MyApp.Image.resize(image, 300, 300),
         {:ok, url} <- MyApp.Storage.upload(resized) do
      {:ok, url}
    else
      {:error, :enoent} -> {:error, :file_missing}
      {:error, :eacces} -> {:error, :cannot_read_file}
      {:error, :unknown_format} -> {:error, :invalid_image}
      {:error, :too_large} -> {:error, :image_too_large}
      {:error, :unsupported_dimensions} -> {:error, :resize_failed}
      {:error, :timeout} -> {:error, :storage_timeout}
      {:error, {:http_error, status}} -> {:error, {:storage_http_error, status}}
    end
  end
end
```

This `with` statement chains four distinct pipeline stages, but every failure is handled together.
The reader cannot quickly tell which error belongs to which operation.
