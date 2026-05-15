smell_id:using_exceptions_for_control_flow

# Example 03

A catalog module raises when a SKU does not exist, and the caller rescues to decide whether to show an out-of-stock style response.

```elixir
defmodule Catalog do
  def fetch_product!(sku) do
    case sku do
      "A-1" -> %{sku: "A-1", stock: 10}
      "B-2" -> %{sku: "B-2", stock: 0}
      _ -> raise RuntimeError, "unknown sku"
    end
  end
end

defmodule InventoryView do
  def availability_label(sku) do
    try do
      product = Catalog.fetch_product!(sku)

      if product.stock > 0 do
        "available"
      else
        "sold out"
      end
    rescue
      e in RuntimeError ->
        "not found: #{e.message}"
    end
  end
end

# Use examples
InventoryView.availability_label("A-1")
InventoryView.availability_label("Z-9")
```
