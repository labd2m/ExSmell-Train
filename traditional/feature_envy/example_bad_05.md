smell_id:feature_envy

# Example 05

```elixir
defmodule Shipment do
  def get(code) do
    %{
      code: code,
      weight: 8.5,
      zone: 3,
      fragile: true,
      insured: false
    }
  end

  def zone_multiplier(1), do: 1.0
  def zone_multiplier(2), do: 1.2
  def zone_multiplier(3), do: 1.6
end

defmodule ShippingLabel do
  def shipping_cost(code) do
    shipment = Shipment.get(code)

    base = shipment.weight * Shipment.zone_multiplier(shipment.zone)
    fragile_fee = if shipment.fragile, do: 15.0, else: 0.0
    insurance_fee = if shipment.insured, do: 20.0, else: 0.0

    base + fragile_fee + insurance_fee
  end
end
```
