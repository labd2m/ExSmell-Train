smell_id:genserver_envy

# Example 1 - Agent used as an order workflow server

```elixir
defmodule CheckoutState do
  def start_link() do
    Agent.start_link(fn -> %{cart: [], status: :open, total: 0} end, name: __MODULE__)
  end

  def add_item(product, price) do
    Agent.get_and_update(__MODULE__, fn state ->
      new_cart = [%{product: product, price: price} | state.cart]
      new_total = state.total + price

      {:ok, %{state | cart: new_cart, total: new_total}}
    end)
  end

  def apply_discount(percent) do
    Agent.get_and_update(__MODULE__, fn state ->
      discounted = state.total - state.total * percent / 100
      {:ok, %{state | total: discounted}}
    end)
  end

  def lock() do
    Agent.update(__MODULE__, fn state ->
      %{state | status: :locked}
    end)
  end

  def unlock() do
    Agent.update(__MODULE__, fn state ->
      %{state | status: :open}
    end)
  end

  def checkout() do
    Agent.get_and_update(__MODULE__, fn state ->
      case state.status do
        :open ->
          receipt = %{items: Enum.reverse(state.cart), total: state.total}
          {{:ok, receipt}, %{state | status: :closed}}

        :locked ->
          {{:error, :checkout_locked}, state}

        :closed ->
          {{:error, :already_checked_out}, state}
      end
    end)
  end
end
```

This is a smell because the `Agent` is being used as a generic server with multiple commands, state transitions, and branching business rules. It is no longer just exposing shared state.
