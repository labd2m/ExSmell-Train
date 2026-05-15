smell_id:comments

# Example 06

```elixir
defmodule Notifications.EmailSubject do
  # Generates the email subject for a delivery notification.
  # If the package has shipped, includes the tracking code.
  # Otherwise informs that the order is still processing.
  def build(order) do
    if order.tracking_code do
      "Your order shipped - #{order.tracking_code}"
    else
      "Your order is being processed"
    end
  end
end
```
