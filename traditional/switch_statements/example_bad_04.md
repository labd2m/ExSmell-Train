smell_id:switch_statements

# Example 04

```elixir
defmodule Roles do
  def dashboard_path(%{role: :admin}), do: "/admin"
  def dashboard_path(%{role: :manager}), do: "/manager"
  def dashboard_path(%{role: :customer}), do: "/home"
end

defmodule Permissions do
  def can_refund?(%{role: :admin}), do: true
  def can_refund?(%{role: :manager}), do: true
  def can_refund?(%{role: :customer}), do: false
end

defmodule Sidebar do
  def menu_items(%{role: :admin}), do: ["users", "orders", "settings"]
  def menu_items(%{role: :manager}), do: ["orders", "reports"]
  def menu_items(%{role: :customer}), do: ["shop", "orders"]
end
```

The role-based decision tree is duplicated across several modules. Adding a `:support` role would require coordinated changes everywhere.
