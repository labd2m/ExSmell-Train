smell_id:divergent_change

# Example 06

```elixir
defmodule Travel.Booking do
  @moduledoc """
  Booking lifecycle, loyalty points, invoice generation,
  and cancellation policies live in the same module.
  """

  # Reason to change: cancellation rules
  def cancellation_fee(%{check_in: check_in, total: total}) do
    days = Date.diff(check_in, Date.utc_today())

    cond do
      days >= 14 -> 0
      days >= 7 -> total * 0.1
      true -> total * 0.3
    end
  end

  # Reason to change: loyalty program rules
  def award_points(user, booking_total) do
    multiplier = if user.tier == :gold, do: 2, else: 1
    trunc(booking_total / 10) * multiplier
  end

  # Reason to change: invoice rendering format
  def invoice_number(id, inserted_at) do
    "BK-#{inserted_at.year}-#{String.pad_leading(to_string(id), 6, "0")}"
  end

  # Reason to change: email itinerary wording
  def itinerary_subject(booking) do
    "Your trip to #{booking.destination}"
  end
end
```
