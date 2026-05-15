smell_id:genserver_envy

# Example 8 - Task used as a cache server with invalidation

```elixir
defmodule ProfileCache do
  def start_link() do
    Task.start_link(fn -> loop(%{}) end)
  end

  def put(pid, id, profile) do
    send(pid, {:put, id, profile})
  end

  def invalidate(pid, id) do
    send(pid, {:invalidate, id})
  end

  def fetch(pid, id) do
    ref = make_ref()
    send(pid, {:fetch, self(), ref, id})

    receive do
      {^ref, result} -> result
    after
      1_000 -> {:error, :timeout}
    end
  end

  defp loop(cache) do
    receive do
      {:put, id, profile} ->
        loop(Map.put(cache, id, %{profile: profile, inserted_at: System.system_time(:second)}))

      {:invalidate, id} ->
        loop(Map.delete(cache, id))

      {:fetch, caller, ref, id} ->
        reply =
          case Map.fetch(cache, id) do
            {:ok, %{inserted_at: inserted_at} = entry} ->
              if System.system_time(:second) - inserted_at < 300 do
                {:ok, entry.profile}
              else
                :stale
              end

            :error ->
              :missing
          end

        send(caller, {ref, reply})
        loop(cache)
    end
  end
end
```

The `Task` has become a stateful cache server with protocol messages, fetch semantics, and expiry rules, which is a typical `GenServer` role.
