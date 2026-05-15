smell_id:complex_extractions_in_clauses

# Example 08 - Ride acceptance rules with several extracted pieces

```elixir
defmodule Mobility.Driver do
  defstruct [:name, :rating, :vehicle_type, :city, :online]
end

defmodule Mobility.Ride do
  defstruct [:id, :distance_km, :passengers, :city, :surge]
end

defmodule Mobility.Matcher do
  alias Mobility.Driver
  alias Mobility.Ride

  def accept?(
        %Driver{name: name, rating: rating, vehicle_type: vehicle_type, city: driver_city, online: online},
        %Ride{id: ride_id, distance_km: distance_km, passengers: passengers, city: ride_city, surge: surge}
      )
      when online == true and rating >= 4.8 and vehicle_type == :premium and driver_city == ride_city and passengers <= 4 do
    {:accept, "#{name} accepts ride #{ride_id} with surge #{surge} for #{distance_km} km"}
  end

  def accept?(
        %Driver{name: name, rating: rating, vehicle_type: vehicle_type, city: driver_city, online: online},
        %Ride{id: ride_id, distance_km: distance_km, passengers: passengers, city: ride_city, surge: surge}
      )
      when online == true and vehicle_type == :standard and driver_city == ride_city and passengers <= 4 do
    {:accept, "#{name} accepts standard ride #{ride_id}; rating=#{rating}; distance=#{distance_km}; surge=#{surge}"}
  end

  def accept?(
        %Driver{name: name, rating: rating, vehicle_type: vehicle_type, city: driver_city, online: online},
        %Ride{id: ride_id, distance_km: distance_km, passengers: passengers, city: ride_city, surge: surge}
      )
      when online == false or driver_city != ride_city or passengers > 4 do
    {:reject, "#{name} rejects ride #{ride_id}; online=#{online}; driver_city=#{driver_city}; ride_city=#{ride_city}; passengers=#{passengers}; vehicle=#{vehicle_type}; rating=#{rating}; surge=#{surge}; distance=#{distance_km}"}
  end
end
```

This kind of clause gets noisy quickly: many extracted values are included only to build the result, while a different subset controls the match.
