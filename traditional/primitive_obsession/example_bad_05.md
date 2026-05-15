smell_id:primitive_obsession

# Primitive Obsession - Example 05

```elixir
defmodule Booking do
  @moduledoc false

  # Dates and time ranges are plain strings instead of richer value objects.
  def reserve(room_id, guest_name, check_in, check_out, timezone, breakfast, late_checkout) do
    %{
      room_id: room_id,
      guest_name: guest_name,
      period: "#{check_in} -> #{check_out} #{timezone}",
      breakfast: breakfast,
      late_checkout: late_checkout
    }
  end
end

# ...Use examples...

Booking.reserve("A-101", "Luisa", "2026-06-01", "2026-06-05", "America/Sao_Paulo", true, false)
Booking.reserve("A-101", "Luisa", "next monday", "sometime later", :utc, "yes", 0)
```
