smell_id:shotgun_surgery

# Shotgun Surgery - Example 09

```elixir
defmodule CourierPrices do
  def price(:dhl, kg), do: 20 + kg * 3
  def price(:fedex, kg), do: 18 + kg * 4
  # def price(:ups, kg), do: 22 + kg * 5
end

defmodule CourierDeadlines do
  def eta(:dhl), do: 2
  def eta(:fedex), do: 3
  # def eta(:ups), do: 4
end

defmodule CourierTracking do
  def tracking_url(:dhl, code), do: "https://dhl.example/track/" <> code
  def tracking_url(:fedex, code), do: "https://fedex.example/track/" <> code
  # def tracking_url(:ups, code), do: "https://ups.example/track/" <> code
end

defmodule ShipmentQuote do
  def quote(courier, kg, tracking_code) do
    %{
      price: CourierPrices.price(courier, kg),
      eta_days: CourierDeadlines.eta(courier),
      tracking: CourierTracking.tracking_url(courier, tracking_code)
    }
  end
end
```
