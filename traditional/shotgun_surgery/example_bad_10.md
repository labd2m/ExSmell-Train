smell_id:shotgun_surgery

# Shotgun Surgery - Example 10

```elixir
defmodule MembershipBadges do
  def badge(:bronze), do: "bronze.png"
  def badge(:silver), do: "silver.png"
  # def badge(:gold), do: "gold.png"
end

defmodule MembershipPoints do
  def multiplier(:bronze), do: 1
  def multiplier(:silver), do: 2
  # def multiplier(:gold), do: 4
end

defmodule MembershipSupport do
  def queue(:bronze), do: :standard
  def queue(:silver), do: :priority
  # def queue(:gold), do: :vip
end

defmodule LoyaltyProgram do
  def benefits(level, purchase_total) do
    %{
      badge: MembershipBadges.badge(level),
      earned_points: trunc(purchase_total * MembershipPoints.multiplier(level)),
      support_queue: MembershipSupport.queue(level)
    }
  end
end
```
