smell_id:switch_statements

# Example 08

```elixir
defmodule DeliveryStatus do
  def humanize(:pending), do: "Pending"
  def humanize(:packed), do: "Packed"
  def humanize(:shipped), do: "Shipped"
  def humanize(:delivered), do: "Delivered"
end

defmodule DeliveryColor do
  def color(:pending), do: :yellow
  def color(:packed), do: :blue
  def color(:shipped), do: :purple
  def color(:delivered), do: :green
end

defmodule DeliveryActions do
  def next_action(:pending), do: :pack
  def next_action(:packed), do: :ship
  def next_action(:shipped), do: :confirm_delivery
  def next_action(:delivered), do: :archive
end
```

The exact same status switching is scattered through several modules. Adding a new status such as `:returned` would require multiple duplicated changes.
