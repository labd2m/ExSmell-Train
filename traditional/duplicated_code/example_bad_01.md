smell_id:duplicated_code

# Example 01

```elixir
defmodule Shop.Location do
  def calculate(zip_code), do: if(String.starts_with?(zip_code, "9"), do: 2.0, else: 1.0)
end

defmodule Shop.ShoppingCart do
  def calculate_shipping(zip_code, subscription) do
    if Enum.member?([3, 4], subscription.id) do
      0.0
    else
      10.0 * Shop.Location.calculate(zip_code)
    end
  end

  def apply_discount(total, subscription) do
    if Enum.member?([3, 4], subscription.id) do
      total * 0.90
    else
      total
    end
  end

  def free_gift?(subscription) do
    if Enum.member?([3, 4], subscription.id) do
      true
    else
      false
    end
  end
end
```
