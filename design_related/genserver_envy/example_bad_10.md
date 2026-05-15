smell_id:genserver_envy

# Example 10 - Task used as a background importer with a command API

```elixir
defmodule ImportRunner do
  def start_link(path) do
    Task.start_link(fn ->
      loop(%{path: path, status: :idle, imported: 0, failed: 0})
    end)
  end

  def start_import(pid) do
    send(pid, :start_import)
  end

  def reset(pid) do
    send(pid, :reset)
  end

  def stats(pid) do
    send(pid, {:stats, self()})

    receive do
      {:stats_reply, data} -> data
    after
      1_000 -> {:error, :timeout}
    end
  end

  defp loop(state) do
    receive do
      :start_import ->
        new_state =
          state.path
          |> File.stream!()
          |> Enum.reduce(%{state | status: :running}, fn line, acc ->
            case import_line(line) do
              :ok -> %{acc | imported: acc.imported + 1}
              {:error, _} -> %{acc | failed: acc.failed + 1}
            end
          end)

        loop(%{new_state | status: :finished})

      :reset ->
        loop(%{state | status: :idle, imported: 0, failed: 0})

      {:stats, caller} ->
        send(caller, {:stats_reply, Map.take(state, [:status, :imported, :failed])})
        loop(state)
    end
  end

  defp import_line(_line), do: :ok
end
```

This `Task` is not a one-off asynchronous computation. It exposes commands, keeps evolving state, and behaves like a server process.
