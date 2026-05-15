smell_id:genserver_envy

# Example 3 - Task used as a long-lived message loop

```elixir
defmodule ChatWorker do
  def start_link(room_id) do
    Task.start_link(fn ->
      loop(%{room_id: room_id, users: MapSet.new(), messages: []})
    end)
  end

  def join(pid, user) do
    send(pid, {:join, user})
  end

  def leave(pid, user) do
    send(pid, {:leave, user})
  end

  def post(pid, user, body) do
    send(pid, {:post, user, body})
  end

  def history(pid) do
    send(pid, {:history, self()})

    receive do
      {:history_reply, messages} -> messages
    after
      1_000 -> {:error, :timeout}
    end
  end

  defp loop(state) do
    receive do
      {:join, user} ->
        loop(%{state | users: MapSet.put(state.users, user)})

      {:leave, user} ->
        loop(%{state | users: MapSet.delete(state.users, user)})

      {:post, user, body} ->
        message = %{user: user, body: body}
        loop(%{state | messages: [message | state.messages]})

      {:history, caller} ->
        send(caller, {:history_reply, Enum.reverse(state.messages)})
        loop(state)
    end
  end
end
```

A `Task` is supposed to run an isolated asynchronous action. Here it is being turned into a permanent server process with state, commands, and a custom protocol.
