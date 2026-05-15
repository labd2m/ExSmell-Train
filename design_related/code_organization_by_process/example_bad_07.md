smell_id:code_organization_by_process

# Example 07 - Currency conversion through a single named process

```elixir
defmodule CurrencyConverter do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, :ok, name: __MODULE__)
  end

  def usd_to_eur(amount), do: GenServer.call(__MODULE__, {:usd_to_eur, amount})
  def eur_to_brl(amount), do: GenServer.call(__MODULE__, {:eur_to_brl, amount})
  def brl_to_usd(amount), do: GenServer.call(__MODULE__, {:brl_to_usd, amount})

  def init(:ok), do: {:ok, nil}

  def handle_call({:usd_to_eur, amount}, _from, state), do: {:reply, amount * 0.92, state}
  def handle_call({:eur_to_brl, amount}, _from, state), do: {:reply, amount * 6.10, state}
  def handle_call({:brl_to_usd, amount}, _from, state), do: {:reply, amount / 5.60, state}
end

{:ok, _pid} = CurrencyConverter.start_link()
CurrencyConverter.usd_to_eur(100)
CurrencyConverter.eur_to_brl(50)
```
