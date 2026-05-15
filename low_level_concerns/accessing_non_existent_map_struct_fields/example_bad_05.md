smell_id:accessing_non_existent_map_struct_fields

# Example 05 - JSON payload inspection after decoding

```elixir
defmodule Importer do
  def import_user(payload) do
    with {:ok, decoded} <- Jason.decode(payload) do
      {:ok, %{name: decoded["name"], email: decoded["email"], age: decoded["age"]}}
    end
  end
end

# Use examples
# iex> Importer.import_user(~s({"name":"Ana","email":"ana@example.com"}))
# {:ok, %{name: "Ana", email: "ana@example.com", age: nil}}
#
# iex> Importer.import_user(~s({"name":"Ana","email":"ana@example.com","age":null}))
# {:ok, %{name: "Ana", email: "ana@example.com", age: nil}}
```

For the decoded map, dynamic access hides whether `"age"` is absent or explicitly null in the source payload.
