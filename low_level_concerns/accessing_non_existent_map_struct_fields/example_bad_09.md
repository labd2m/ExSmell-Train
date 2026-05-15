smell_id:accessing_non_existent_map_struct_fields

# Example 09 - CSV row mapping with ambiguous columns

```elixir
defmodule CsvRowMapper do
  def to_product(row) do
    %{
      sku: row[:sku],
      name: row[:name],
      category: row[:category]
    }
  end
end

# Use examples
# iex> CsvRowMapper.to_product(%{sku: "A1", name: "Keyboard"})
# %{sku: "A1", name: "Keyboard", category: nil}
#
# iex> CsvRowMapper.to_product(%{sku: "A1", name: "Keyboard", category: nil})
# %{sku: "A1", name: "Keyboard", category: nil}
```

The mapper produces the same result for a missing CSV column and for an existing column explicitly parsed as `nil`.
