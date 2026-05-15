smell_id:genserver_envy

# Example 7 - Agent used to run domain commands instead of just sharing state

```elixir
defmodule InventoryAgent do
  def start_link() do
    Agent.start_link(fn -> %{stock: %{}, reservations: %{}} end, name: __MODULE__)
  end

  def add_sku(sku, qty) do
    Agent.update(__MODULE__, fn state ->
      update_in(state.stock[sku], fn current -> (current || 0) + qty end)
    end)
  end

  def reserve(order_id, sku, qty) do
    Agent.get_and_update(__MODULE__, fn state ->
      available = Map.get(state.stock, sku, 0)

      if available >= qty do
        reservations = Map.put(state.reservations, order_id, %{sku: sku, qty: qty})
        new_stock = Map.put(state.stock, sku, available - qty)
        {{:ok, :reserved}, %{state | stock: new_stock, reservations: reservations}}
      else
        {{:error, :out_of_stock}, state}
      end
    end)
  end

  def release(order_id) do
    Agent.get_and_update(__MODULE__, fn state ->
      case Map.pop(state.reservations, order_id) do
        {nil, _reservations} ->
          {{:error, :missing_reservation}, state}

        {%{sku: sku, qty: qty}, reservations} ->
          new_stock = Map.update(state.stock, sku, qty, &(&1 + qty))
          {{:ok, :released}, %{state | stock: new_stock, reservations: reservations}}
      end
    end)
  end
end
```

This `Agent` embeds a business protocol with multiple commands and state transitions. It is being treated like a generic stateful server.
