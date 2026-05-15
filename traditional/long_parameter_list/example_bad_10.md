smell_id:long_parameter_list

```elixir
defmodule ProductCatalog do
  def create_product(name, sku, description, category, price_cents, currency, stock, published, featured) do
    %{
      name: name,
      sku: sku,
      description: description,
      category: category,
      price: {price_cents, currency},
      stock: stock,
      published: published,
      featured: featured
    }
  end
end

ProductCatalog.create_product(
  "Mechanical Keyboard",
  "KB-001",
  "Compact wireless keyboard",
  "accessories",
  35900,
  "BRL",
  25,
  true,
  false
)
```
