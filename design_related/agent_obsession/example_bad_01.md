smell_id:agent_obsession

# Example 01 — Shopping cart state updated from many modules

```elixir
defmodule Shop.CartStore do
  def start_link do
    Agent.start_link(fn -> %{items: [], coupon: nil, checked_out: false} end, name: __MODULE__)
  end
end

defmodule Shop.CartItems do
  def add_item(product_id, qty) do
    Agent.update(Shop.CartStore, fn state ->
      update_in(state[:items], fn items -> [%{product_id: product_id, qty: qty} | items] end)
    end)
  end

  def remove_item(product_id) do
    Agent.update(Shop.CartStore, fn state ->
      items = Enum.reject(state.items, fn item -> item.product_id == product_id end)
      %{state | items: items}
    end)
  end
end

defmodule Shop.CouponEngine do
  def apply_coupon(code) do
    Agent.update(Shop.CartStore, fn state ->
      Map.put(state, :coupon, String.upcase(code))
    end)
  end
end

defmodule Shop.Checkout do
  def mark_checked_out do
    Agent.update(Shop.CartStore, fn state ->
      %{state | checked_out: true}
    end)
  end
end

defmodule Shop.CartSummary do
  def total_items do
    Agent.get(Shop.CartStore, fn state ->
      Enum.reduce(state.items, 0, fn item, acc -> acc + item.qty end)
    end)
  end

  def snapshot do
    Agent.get(Shop.CartStore, & &1)
  end
end
```

Here the responsibility for reading and mutating the shared cart `Agent` is spread across multiple modules. No single boundary controls the shape of the state or the allowed transitions.
