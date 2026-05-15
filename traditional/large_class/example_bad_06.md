smell_id:large_class

# Example 06

```elixir
defmodule TravelBooking do
  alias Travel.Gateway
  alias Travel.Mailer

  # Search
  def search_flights(origin, destination, date) do
    [{origin, destination, date}]
  end

  def search_hotels(city, from, to) do
    [{city, from, to}]
  end

  # Pricing
  def calculate_trip_total(flight_price, hotel_price, insurance) do
    flight_price + hotel_price + insurance
  end

  def apply_loyalty_discount(total, :gold), do: total * 0.9
  def apply_loyalty_discount(total, :silver), do: total * 0.95
  def apply_loyalty_discount(total, _), do: total

  # Booking
  def issue_ticket(booking) do
    Gateway.issue_ticket(booking)
  end

  def reserve_room(booking) do
    Gateway.reserve_room(booking)
  end

  # Refunds
  def refund_ticket(ticket_id) do
    Gateway.refund(ticket_id)
  end

  # Itinerary formatting
  def print_itinerary(user, booking) do
    "Trip for #{user.name}: #{inspect(booking)}"
  end

  # Notifications
  def send_confirmation(user, booking) do
    Mailer.send(user.email, "Booking confirmed: #{inspect(booking)}")
  end
end
```
