smell_id:long_parameter_list

```elixir
defmodule Shipping do
  def quote(weight_kg, height_cm, width_cm, depth_cm, origin_zip, destination_zip, service_level, insured) do
    base = weight_kg * 100
    distance_factor = if origin_zip == destination_zip, do: 1, else: 1.4
    insurance = if insured, do: 500, else: 0

    %{ 
      package: {height_cm, width_cm, depth_cm},
      from: origin_zip,
      to: destination_zip,
      service_level: service_level,
      total_cents: trunc(base * distance_factor) + insurance
    }
  end
end

Shipping.quote(2.5, 30, 20, 10, "30110-000", "01000-000", :express, true)
```
