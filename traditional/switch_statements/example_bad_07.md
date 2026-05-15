smell_id:switch_statements

# Example 07

```elixir
defmodule SubscriptionRenewal do
  def renewal_fee(%{tier: :basic}), do: 20
  def renewal_fee(%{tier: :plus}), do: 50
  def renewal_fee(%{tier: :premium}), do: 90
end

defmodule SubscriptionBenefits do
  def support_level(%{tier: :basic}), do: :community
  def support_level(%{tier: :plus}), do: :priority
  def support_level(%{tier: :premium}), do: :dedicated
end

defmodule SubscriptionEmails do
  def renewal_template(%{tier: :basic}), do: "basic_renewal.html"
  def renewal_template(%{tier: :plus}), do: "plus_renewal.html"
  def renewal_template(%{tier: :premium}), do: "premium_renewal.html"
end
```

The same tier-based switching is repeated in multiple modules. If the business adds a `:vip` tier, all these branches must be updated together.
