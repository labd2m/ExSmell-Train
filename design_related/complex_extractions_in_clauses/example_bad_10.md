smell_id:complex_extractions_in_clauses

# Example 10 - Inventory fulfillment with multiple clauses and body-only values in the pattern

```elixir
defmodule Warehouse.Item do
  defstruct [:sku, :name, :stock, :fragile, :warehouse, :weight_kg]
end

defmodule Warehouse.Fulfillment do
  alias Warehouse.Item

  def pick(%Item{
        sku: sku,
        name: name,
        stock: stock,
        fragile: fragile,
        warehouse: warehouse,
        weight_kg: weight_kg
      })
      when stock > 0 and fragile == true and weight_kg < 10 do
    {:manual_pick, "Pick fragile item #{sku} - #{name} from #{warehouse}; stock=#{stock}"}
  end

  def pick(%Item{
        sku: sku,
        name: name,
        stock: stock,
        fragile: fragile,
        warehouse: warehouse,
        weight_kg: weight_kg
      })
      when stock > 0 and fragile == false and weight_kg < 25 do
    {:belt_pick, "Pick regular item #{sku} - #{name} from #{warehouse}; stock=#{stock}"}
  end

  def pick(%Item{
        sku: sku,
        name: name,
        stock: stock,
        fragile: fragile,
        warehouse: warehouse,
        weight_kg: weight_kg
      })
      when stock <= 0 or weight_kg >= 25 do
    {:special_case, "Cannot use regular flow for #{sku} - #{name}; stock=#{stock}; fragile=#{fragile}; warehouse=#{warehouse}; weight=#{weight_kg}"}
  end
end
```

This is a good illustration of the smell because the clause patterns extract every field, even though only some are important for the guards and others are body-only details.
