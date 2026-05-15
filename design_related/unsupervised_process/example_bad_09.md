smell_id:unsupervised_process

# Example 09 - Background notification processes started from a Phoenix channel

```elixir
defmodule Notify.Dispatcher do
  use GenServer

  def start(user_id) do
    GenServer.start(__MODULE__, %{user_id: user_id, queue: []})
  end

  def enqueue(pid, notification) do
    GenServer.cast(pid, {:enqueue, notification})
  end

  @impl true
  def init(state), do: {:ok, state}

  @impl true
  def handle_cast({:enqueue, notification}, state) do
    {:noreply, %{state | queue: [notification | state.queue]}}
  end
end

defmodule NotifyWeb.UserChannel do
  def join("user:" <> user_id, _payload, socket) do
    # Every joined socket spawns a long-lived dispatcher process.
    # The process is not tracked by any DynamicSupervisor.
    {:ok, pid} = Notify.Dispatcher.start(user_id)
    Notify.Dispatcher.enqueue(pid, %{kind: :welcome})
    {:ok, Map.put(socket, :dispatcher_pid, pid)}
  end
end
```
