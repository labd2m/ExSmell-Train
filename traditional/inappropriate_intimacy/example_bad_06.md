smell_id:inappropriate_intimacy

# Example 06 - Cart reaching into coupon rules

```elixir
defmodule Coupon do
  defstruct [:code, :starts_at, :ends_at, :minimum_total, :percent_off]
end

defmodule ShoppingCart do
  def apply_coupon(total, %Coupon{} = coupon, now) do
    if now >= coupon.starts_at and now <= coupon.ends_at and total >= coupon.minimum_total do
      total - total * coupon.percent_off / 100
    else
      total
    end
  end
end
```

`ShoppingCart.apply_coupon/3` directly interprets the lifecycle and discount rules of `Coupon`.
