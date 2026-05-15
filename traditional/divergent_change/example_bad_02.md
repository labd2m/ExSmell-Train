smell_id:divergent_change

# Example 02

```elixir
defmodule Shop.Inventory do
  @moduledoc """
  The same module mixes inventory control, pricing policy,
  supplier synchronization, and CSV export.
  """

  # Reason to change: stock rules
  def reserve_stock(product_id, amount) do
    product = Repo.get!(Product, product_id)
    new_stock = product.stock - amount
    Repo.update!(Ecto.Changeset.change(product, stock: new_stock))
  end

  # Reason to change: pricing strategy
  def calculate_sale_price(product, season) do
    base = product.base_price

    case season do
      :black_friday -> base * 0.7
      :summer -> base * 0.9
      _ -> base
    end
  end

  # Reason to change: supplier API integration
  def sync_with_supplier(product) do
    payload = %{sku: product.sku, stock: product.stock}
    SupplierClient.post_stock(payload)
  end

  # Reason to change: export format
  def export_csv(products) do
    header = "sku,name,stock,price
"

    body =
      Enum.map_join(products, "
", fn p ->
        "#{p.sku},#{p.name},#{p.stock},#{p.base_price}"
      end)

    header <> body
  end
end
```
