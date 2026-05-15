smell_id:genserver_envy

# Example 4 - Task used as a scheduler and command processor

```elixir
defmodule EmailDispatcher do
  def start_link() do
    Task.start_link(fn ->
      loop(%{queue: :queue.new(), paused: false})
    end)
  end

  def enqueue(pid, email) do
    send(pid, {:enqueue, email})
  end

  def pause(pid) do
    send(pid, :pause)
  end

  def resume(pid) do
    send(pid, :resume)
  end

  def status(pid) do
    send(pid, {:status, self()})

    receive do
      {:status_reply, state} -> state
    after
      500 -> {:error, :timeout}
    end
  end

  defp loop(state) do
    receive do
      {:enqueue, email} ->
        loop(%{state | queue: :queue.in(email, state.queue)})

      :pause ->
        loop(%{state | paused: true})

      :resume ->
        loop(%{state | paused: false})

      {:status, caller} ->
        send(caller, {:status_reply, %{paused: state.paused, queue_size: :queue.len(state.queue)}})
        loop(state)
    after
      1_000 ->
        case {state.paused, :queue.out(state.queue)} do
          {false, {{:value, email}, rest}} ->
            Mailer.deliver(email)
            loop(%{state | queue: rest})

          _ ->
            loop(state)
        end
    end
  end
end

defmodule Mailer do
  def deliver(email) do
    IO.inspect({:delivered, email})
  end
end
```

The `Task` is acting like a server with periodic work, mutable state, a message protocol, and sync-style replies. That is `GenServer` territory.
