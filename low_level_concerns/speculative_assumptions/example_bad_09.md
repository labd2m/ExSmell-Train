smell_id:speculative_assumptions

# Example 09 - Coordinate extraction from loosely formatted strings

```elixir
defmodule GeoPoint do
  @doc """
  Extracts the longitude from a `lat,long` string.

  The code assumes there are exactly two comma-separated pieces.
  """
  def longitude(coords) do
    coords
    |> String.split(",")
    |> Enum.at(1)
  end
end

# Planned input
GeoPoint.longitude("-19.9,-43.9")
# => "-43.9"

# Unplanned input silently returns a label instead of a coordinate
GeoPoint.longitude("-19.9,long=-43.9,alt=800")
# => "long=-43.9"
```
