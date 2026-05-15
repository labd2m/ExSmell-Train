smell_id:shotgun_surgery

# Shotgun Surgery - Example 01

```elixir
defmodule ShippingRules do
  def shipping_cost(_zip, %{tier: :gold}), do: 0
  def shipping_cost(_zip, %{tier: :platinum}), do: 0
  # def shipping_cost(_zip, %{tier: :diamond}), do: 0  # <= another tiny edit

  def shipping_cost(zip, _customer) do
    12 * Zone.multiplier(zip)
  end
end

defmodule DiscountRules do
  def discount(total, %{tier: :gold}), do: total * 0.10
  def discount(total, %{tier: :platinum}), do: total * 0.15
  # def discount(total, %{tier: :diamond}), do: total * 0.20

  def discount(_total, _customer), do: 0
end

defmodule RenewalFees do
  def fee(%{tier: :gold}), do: 29
  def fee(%{tier: :platinum}), do: 19
  # def fee(%{tier: :diamond}), do: 9

  def fee(_customer), do: 49
end
```
