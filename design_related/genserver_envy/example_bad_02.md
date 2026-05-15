smell_id:genserver_envy

# Example 2 - Agent used to encapsulate state and side effects

```elixir
defmodule AuditBuffer do
  def start_link(file_path) do
    Agent.start_link(fn -> %{file_path: file_path, entries: []} end, name: __MODULE__)
  end

  def append(entry) do
    Agent.update(__MODULE__, fn state ->
      %{state | entries: [entry | state.entries]}
    end)
  end

  def flush() do
    Agent.get_and_update(__MODULE__, fn state ->
      File.write!(state.file_path, Enum.join(Enum.reverse(state.entries), "\n") <> "\n", [:append])
      {:ok, %{state | entries: []}}
    end)
  end

  def flush_if_large() do
    Agent.get_and_update(__MODULE__, fn state ->
      if length(state.entries) > 100 do
        File.write!(state.file_path, Enum.join(Enum.reverse(state.entries), "\n") <> "\n", [:append])
        {:flushed, %{state | entries: []}}
      else
        {:skipped, state}
      end
    end)
  end
end
```

This `Agent` does much more than hold shared state. It coordinates commands, performs file I/O, and makes control-flow decisions. That behavior is closer to a `GenServer`.
