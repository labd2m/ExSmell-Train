smell_id:unsupervised_process

# Example 08 - Workers started in application start callback but not supervised

```elixir
defmodule Feed.Refresher do
  use GenServer

  def start(source) do
    GenServer.start(__MODULE__, %{source: source})
  end

  @impl true
  def init(state) do
    Process.send_after(self(), :refresh, 1_000)
    {:ok, state}
  end

  @impl true
  def handle_info(:refresh, state) do
    Process.send_after(self(), :refresh, 1_000)
    {:noreply, state}
  end
end

defmodule Feed.Application do
  use Application

  def start(_type, _args) do
    Enum.each(["news", "sports", "finance"], fn source ->
      # These are started during boot, but not attached to the returned supervisor.
      {:ok, _pid} = Feed.Refresher.start(source)
    end)

    children = [
      {Task.Supervisor, name: Feed.TaskSupervisor}
    ]

    Supervisor.start_link(children, strategy: :one_for_one)
  end
end
```
