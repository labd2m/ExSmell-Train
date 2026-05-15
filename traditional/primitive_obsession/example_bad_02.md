smell_id:primitive_obsession

# Primitive Obsession - Example 02

```elixir
defmodule Shipping do
  @moduledoc false

  # A delivery address is represented as many loose strings.
  def ship(order_id, recipient_name, street, number, city, state, zip_code, country) do
    label = """
    Order: #{order_id}
    To: #{recipient_name}
    #{street}, #{number}
    #{city}, #{state} #{zip_code}
    #{country}
    """

    %{order_id: order_id, label: label}
  end
end

# ...Use examples...

Shipping.ship("ORD-9", "Ana", "Main St", "100", "Boston", "MA", "02108", "US")
Shipping.ship("ORD-9", "Ana", "Main St", 100, "Boston", "Massachusetts", 2108, :usa)
```
