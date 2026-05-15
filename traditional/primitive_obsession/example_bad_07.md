smell_id:primitive_obsession

# Primitive Obsession - Example 07

```elixir
defmodule Geo do
  @moduledoc false

  # Latitude and longitude are naked floats with no domain wrapper.
  # radius_unit is another primitive string.
  def within_radius?(lat1, lon1, lat2, lon2, radius, radius_unit) do
    distance = :math.sqrt(:math.pow(lat2 - lat1, 2) + :math.pow(lon2 - lon1, 2))

    case radius_unit do
      "degrees" -> distance <= radius
      "km" -> distance * 111 <= radius
    end
  end
end

# ...Use examples...

Geo.within_radius?(-19.9, -43.9, -19.91, -43.95, 10, "km")
Geo.within_radius?("BH", "MG", -19.91, -43.95, "ten", :kilometers)
```
