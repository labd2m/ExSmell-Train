smell_id:code_organization_by_process

# Example 01 - Calculator wrapped in a GenServer

```elixir
defmodule BillingCalculator do
  use GenServer

  @moduledoc """
  A calculator process used only for code organization.
  No shared mutable state or scheduling is needed.
  """

  def start_link do
    GenServer.start_link(__MODULE__, :ok)
  end

  def subtotal(pid, unit_price, quantity) do
    GenServer.call(pid, {:subtotal, unit_price, quantity})
  end

  def discount(pid, amount, percent) do
    GenServer.call(pid, {:discount, amount, percent})
  end

  @impl true
  def init(:ok), do: {:ok, nil}

  @impl true
  def handle_call({:subtotal, unit_price, quantity}, _from, state) do
    {:reply, unit_price * quantity, state}
  end

  def handle_call({:discount, amount, percent}, _from, state) do
    {:reply, amount - amount * percent / 100, state}
  end
end

{:ok, pid} = BillingCalculator.start_link()
BillingCalculator.subtotal(pid, 12, 5)
BillingCalculator.discount(pid, 100, 10)
```
