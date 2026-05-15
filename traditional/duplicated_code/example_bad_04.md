smell_id:duplicated_code

# Example 04

```elixir
defmodule Importer.CSV do
  def import_row(row) do
    case Integer.parse(row["quantity"] || "") do
      {quantity, ""} when quantity > 0 ->
        {:ok, %{sku: row["sku"], quantity: quantity}}

      _ ->
        {:error, :invalid_quantity}
    end
  end

  def preview_row(row) do
    case Integer.parse(row["quantity"] || "") do
      {quantity, ""} when quantity > 0 ->
        {:ok, %{sku: row["sku"], quantity: quantity}}

      _ ->
        {:error, :invalid_quantity}
    end
  end

  def normalize_row(row) do
    case Integer.parse(row["quantity"] || "") do
      {quantity, ""} when quantity > 0 ->
        {:ok, %{sku: row["sku"], quantity: quantity}}

      _ ->
        {:error, :invalid_quantity}
    end
  end
end
```
