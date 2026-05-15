smell_id:duplicated_code

# Example 10

```elixir
defmodule Checkout.Shipping do
  def express_available?(order) do
    order.country in ["US", "CA"] and order.weight_kg < 20 and order.contains_hazmat == false
  end
end

defmodule Checkout.PriceRules do
  def express_surcharge(order) do
    if order.country in ["US", "CA"] and order.weight_kg < 20 and order.contains_hazmat == false do
      12.50
    else
      0.0
    end
  end
end

defmodule Checkout.Labels do
  def shipping_label(order) do
    if order.country in ["US", "CA"] and order.weight_kg < 20 and order.contains_hazmat == false do
      "EXPRESS"
    else
      "STANDARD"
    end
  end
end
```
