smell_id:unsupervised_process

# Example 02 - Chat rooms started dynamically without a DynamicSupervisor

```elixir
defmodule Chat.Room do
  use GenServer

  def start(room_name) do
    GenServer.start(__MODULE__, %{room: room_name, messages: []}, name: via(room_name))
  end

  def post(room_name, message) do
    GenServer.cast(via(room_name), {:post, message})
  end

  defp via(room_name), do: {:global, {:chat_room, room_name}}

  @impl true
  def init(state), do: {:ok, state}

  @impl true
  def handle_cast({:post, message}, state) do
    {:noreply, %{state | messages: [message | state.messages]}}
  end
end

defmodule Chat.RoomManager do
  def ensure_room(room_name) do
    case :global.whereis_name({:chat_room, room_name}) do
      :undefined ->
        # Potentially many room processes are started directly here.
        # They are long-lived, but not supervised anywhere.
        Chat.Room.start(room_name)

      pid when is_pid(pid) ->
        {:ok, pid}
    end
  end
end
```
