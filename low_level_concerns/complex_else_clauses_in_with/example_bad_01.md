smell_id:complex_else_clauses_in_with

# Example 01

```elixir
defmodule MyApp.FileDecoder do
  def open_decoded_file(path) do
    with {:ok, encoded} <- File.read(path),
         {:ok, decoded} <- Base.decode64(encoded) do
      decoded
    else
      {:error, :enoent} -> :missing_file
      {:error, :eacces} -> :permission_denied
      {:error, :enomem} -> :filesystem_failure
      {:error, _reason} -> :unreadable_file
      :error -> :invalid_base64
    end
  end
end
```

The `with` expression flattens file reading errors and decoding errors into one `else` block.
A reader must mentally track which clauses can return tuples and which can return atoms.
