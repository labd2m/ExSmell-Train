smell_id:large_class

# Example 03

```elixir
defmodule Inventory do
  alias Warehouse.SupplierAPI
  alias Warehouse.CSVExporter

  # Stock control
  def in_stock?(sku), do: lookup_stock(sku) > 0
  def reserve(sku, quantity), do: {:ok, {sku, quantity}}
  def restock(sku, quantity), do: {:ok, {sku, quantity}}

  # Pricing
  def calculate_margin(cost, price) do
    (price - cost) / cost
  end

  def apply_bulk_price(price, quantity) when quantity > 100, do: price * 0.8
  def apply_bulk_price(price, quantity) when quantity > 10, do: price * 0.9
  def apply_bulk_price(price, _quantity), do: price

  # Supplier integration
  def sync_supplier_catalog(supplier_id) do
    SupplierAPI.fetch_catalog(supplier_id)
  end

  def place_supplier_order(supplier_id, lines) do
    SupplierAPI.place_order(supplier_id, lines)
  end

  # Reporting
  def export_low_stock_report() do
    CSVExporter.export("low-stock.csv", low_stock_items())
  end

  # Forecasting
  def forecast_needed_units(history) do
    history
    |> Enum.take(-6)
    |> Enum.sum()
    |> Kernel./(6)
    |> round()
  end

  defp lookup_stock(_sku), do: 15
  defp low_stock_items, do: [%{sku: "ABC", qty: 2}]
end
```
