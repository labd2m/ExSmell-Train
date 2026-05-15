smell_id:genserver_envy

# Example 6 - Task used as a registry with custom request-response logic

```elixir
defmodule UserSessionRegistry do
  def start_link() do
    Task.start_link(fn -> loop(%{}) end)
  end

  def register(pid, user_id, session_pid) do
    send(pid, {:register, user_id, session_pid})
  end

  def unregister(pid, user_id) do
    send(pid, {:unregister, user_id})
  end

  def lookup(pid, user_id) do
    ref = make_ref()
    send(pid, {:lookup, self(), ref, user_id})

    receive do
      {^ref, result} -> result
    after
      1_000 -> {:error, :timeout}
    end
  end

  def count(pid) do
    ref = make_ref()
    send(pid, {:count, self(), ref})

    receive do
      {^ref, result} -> result
    after
      1_000 -> {:error, :timeout}
    end
  end

  defp loop(state) do
    receive do
      {:register, user_id, session_pid} ->
        loop(Map.put(state, user_id, session_pid))

      {:unregister, user_id} ->
        loop(Map.delete(state, user_id))

      {:lookup, caller, ref, user_id} ->
        send(caller, {ref, Map.fetch(state, user_id)})
        loop(state)

      {:count, caller, ref} ->
        send(caller, {ref, map_size(state)})
        loop(state)
    end
  end
end
```

The custom mailbox protocol, internal state, and multiple synchronous interactions show that this `Task` is being used as a home-made `GenServer`.
