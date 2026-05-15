smell_id:unsupervised_process

# Example 07 - Feature flag caches started per tenant by a library entrypoint

```elixir
defmodule Flags.Cache do
  use GenServer

  def start(tenant) do
    GenServer.start(__MODULE__, %{tenant: tenant, flags: %{}})
  end

  def put(pid, key, value) do
    GenServer.cast(pid, {:put, key, value})
  end

  @impl true
  def init(state), do: {:ok, state}

  @impl true
  def handle_cast({:put, key, value}, state) do
    {:noreply, %{state | flags: Map.put(state.flags, key, value)}}
  end
end

defmodule Flags do
  def boot_tenants(tenants) do
    for tenant <- tenants do
      # The application is effectively managing its own process fleet
      # outside any supervision hierarchy.
      {:ok, pid} = Flags.Cache.start(tenant)
      Flags.Cache.put(pid, :new_checkout, true)
      {tenant, pid}
    end
  end
end
```
