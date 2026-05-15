smell_id:complex_extractions_in_clauses

# Example 03 - Discount calculation mixing guard data and body-only data

```elixir
defmodule Billing.Customer do
  defstruct [:name, :tier, :region, :age, :coupon]
end

defmodule Billing.Discounts do
  alias Billing.Customer

  def apply_discount(
        amount,
        %Customer{name: name, tier: tier, region: region, age: age, coupon: coupon}
      )
      when amount > 10_000 and tier == :gold and region in ["LATAM", "EU"] do
    {:ok, "#{name} receives premium discount at age #{age} using #{inspect(coupon)}"}
  end

  def apply_discount(
        amount,
        %Customer{name: name, tier: tier, region: region, age: age, coupon: coupon}
      )
      when amount > 5_000 and tier == :silver and age >= 60 do
    {:ok, "#{name} receives senior silver discount for #{region} using #{inspect(coupon)}"}
  end

  def apply_discount(
        amount,
        %Customer{name: name, tier: tier, region: region, age: age, coupon: coupon}
      )
      when amount <= 5_000 or tier == :basic do
    {:none, "#{name} gets no discount in #{region}; age=#{age}; coupon=#{inspect(coupon)}; tier=#{tier}"}
  end
end
```

The smell appears because the clauses extract `name`, `age`, and `coupon` even when they are not part of the matching logic and exist only for formatting the return value.
