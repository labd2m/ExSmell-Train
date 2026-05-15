smell_id:unsupervised_process

# Example 03 - Polling workers started from a service module and left running forever

```elixir
defmodule Metrics.Poller do
  use GenServer

  def start(endpoint) do
    GenServer.start(__MODULE__, endpoint)
  end

  @impl true
  def init(endpoint) do
    schedule_poll()
    {:ok, %{endpoint: endpoint, latest: nil}}
  end

  @impl true
  def handle_info(:poll, state) do
    schedule_poll()
    {:noreply, %{state | latest: {:ok, System.system_time(:second)}}}
  end

  defp schedule_poll do
    Process.send_after(self(), :poll, 5_000)
  end
end

defmodule Metrics.Boot do
  def start_pollers(endpoints) do
    Enum.map(endpoints, fn endpoint ->
      # All pollers become detached long-running processes.
      # A crash or restart strategy is not managed by supervisors.
      {:ok, pid} = Metrics.Poller.start(endpoint)
      pid
    end)
  end
end
```
