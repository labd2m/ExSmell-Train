smell_id:primitive_obsession

# Primitive Obsession - Example 04

```elixir
defmodule Catalog do
  @moduledoc false

  # Price, weight, dimensions, and status are all loose primitives.
  def create_product(name, sku, price, currency, weight, width, height, depth, status) do
    %{
      name: name,
      sku: sku,
      price: price,
      currency: currency,
      shipping_weight: weight,
      dimensions: {width, height, depth},
      status: status
    }
  end
end

# ...Use examples...

Catalog.create_product("Amp", "AMP-1", 499.99, "USD", 15.2, 50, 20, 18, "active")
Catalog.create_product("Amp", "AMP-1", "499.99", :usd, :heavy, "wide", "tall", "deep", true)
```
