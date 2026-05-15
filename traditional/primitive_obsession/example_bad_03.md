smell_id:primitive_obsession

# Primitive Obsession - Example 03

```elixir
defmodule Loyalty do
  @moduledoc false

  # tier is a string and points is an integer without any domain type.
  def upgrade_member(member_id, tier, points, active, expires_in_days) do
    if active and points > 1000 and tier != "vip" do
      %{member_id: member_id, tier: "vip", expires_in_days: expires_in_days}
    else
      %{member_id: member_id, tier: tier, expires_in_days: expires_in_days}
    end
  end
end

# ...Use examples...

Loyalty.upgrade_member("m1", "gold", 1500, true, 30)
Loyalty.upgrade_member("m1", "GOLD", -50, "yes", "thirty")
```
