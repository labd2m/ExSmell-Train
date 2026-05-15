smell_id:feature_envy

# Example 07

```elixir
defmodule Reservation do
  def fetch(id) do
    %{
      id: id,
      nights: 5,
      nightly_rate: 220.0,
      breakfast: true,
      guest_count: 2
    }
  end

  def breakfast_fee_per_guest, do: 25.0
end

defmodule HotelBilling do
  def total(id) do
    reservation = Reservation.fetch(id)

    room_total = reservation.nights * reservation.nightly_rate

    breakfast_total =
      if reservation.breakfast do
        reservation.nights * reservation.guest_count * Reservation.breakfast_fee_per_guest()
      else
        0
      end

    room_total + breakfast_total
  end
end
```
