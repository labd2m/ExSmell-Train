smell_id:inappropriate_intimacy

# Example 03 - Shipping using warehouse implementation details

```elixir
defmodule Warehouse do
  defstruct [:code, :region, :priority_multiplier, :open]
end

defmodule Shipment do
  def calculate_cost(base_cost, %Warehouse{} = warehouse) do
    if warehouse.open do
      if warehouse.region == "international" do
        base_cost * warehouse.priority_multiplier * 3
      else
        base_cost * warehouse.priority_multiplier
      end
    else
      {:error, :warehouse_closed}
    end
  end
end
```

`Shipment.calculate_cost/2` hard-codes rules based on `Warehouse` internals rather than asking `Warehouse` for shipping policy information.
