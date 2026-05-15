smell_id:shotgun_surgery

# Shotgun Surgery - Example 08

```elixir
defmodule RolePermissions do
  def can_edit?(:viewer), do: false
  def can_edit?(:editor), do: true
  # def can_edit?(:reviewer), do: true
end

defmodule RoleLabels do
  def label(:viewer), do: "Read only"
  def label(:editor), do: "Editor"
  # def label(:reviewer), do: "Reviewer"
end

defmodule RoleLandingPage do
  def page(:viewer), do: "/dashboard"
  def page(:editor), do: "/editor"
  # def page(:reviewer), do: "/reviews"
end
```
