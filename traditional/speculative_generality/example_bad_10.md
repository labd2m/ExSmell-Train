smell_id:speculative_generality

# Example 10

```elixir
defmodule DiscountRules do
  def apply(order, customer_type \ :regular) do
    rate =
      case customer_type do
        :regular -> 0.05
        :vip -> 0.05
        :partner -> 0.05
      end

    order.total * (1 - rate)
  end

  def apply_campaign(order, campaign \ nil) do
    case campaign do
      nil -> apply(order)
      _ -> apply(order)
    end
  end
end
```

The module contains speculative branches for customer segmentation and campaigns, but none of them change behavior.
