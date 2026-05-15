smell_id:code_organization_by_process

# Example 03 - Tax rules funneled through one process

```elixir
defmodule TaxEngine do
  use GenServer

  @doc """
  Pure arithmetic rules are serialized through a single server.
  """
  def start_link do
    GenServer.start_link(__MODULE__, :ok, name: __MODULE__)
  end

  def gross_to_net(gross), do: GenServer.call(__MODULE__, {:gross_to_net, gross})
  def add_vat(amount), do: GenServer.call(__MODULE__, {:add_vat, amount})

  def init(:ok), do: {:ok, nil}

  def handle_call({:gross_to_net, gross}, _from, state) do
    {:reply, gross * 0.73, state}
  end

  def handle_call({:add_vat, amount}, _from, state) do
    {:reply, amount * 1.20, state}
  end
end

{:ok, _pid} = TaxEngine.start_link()
TaxEngine.gross_to_net(5000)
TaxEngine.add_vat(150)
```
