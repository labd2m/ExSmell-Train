smell_id:unsupervised_process

# Example 05 - Session timeout loops spawned manually and never supervised

```elixir
defmodule Auth.SessionWatcher do
  def start(session_id) do
    spawn_link(fn -> loop(session_id, System.monotonic_time(:millisecond)) end)
  end

  defp loop(session_id, started_at) do
    receive do
      :refresh ->
        loop(session_id, System.monotonic_time(:millisecond))

      :stop ->
        :ok
    after
      60_000 ->
        IO.puts("Session #{session_id} expired after #{System.monotonic_time(:millisecond) - started_at} ms")
        loop(session_id, started_at)
    end
  end
end

defmodule Auth.SessionService do
  def open_sessions(session_ids) do
    Enum.map(session_ids, fn session_id ->
      # Many long-running watcher processes created ad hoc.
      # There is no supervision tree entry for them.
      Auth.SessionWatcher.start(session_id)
    end)
  end
end
```
