smell_id:agent_obsession

# Example 04 — Import progress state touched by controller, worker, and reporter

```elixir
defmodule Imports.ProgressStore do
  def start_link(import_id) do
    Agent.start_link(fn -> %{id: import_id, status: :pending, total: 0, processed: 0, errors: []} end)
  end
end

defmodule Imports.Controller do
  def mark_started(progress_pid, total_rows) do
    Agent.update(progress_pid, fn state ->
      %{state | status: :running, total: total_rows}
    end)
  end
end

defmodule Imports.RowWorker do
  def mark_processed(progress_pid) do
    Agent.update(progress_pid, fn state ->
      %{state | processed: state.processed + 1}
    end)
  end

  def record_error(progress_pid, row_number, reason) do
    Agent.update(progress_pid, fn state ->
      %{state | errors: [{row_number, reason} | state.errors]}
    end)
  end
end

defmodule Imports.Finalizer do
  def mark_finished(progress_pid) do
    Agent.update(progress_pid, fn state ->
      %{state | status: :finished}
    end)
  end
end

defmodule Imports.Reporter do
  def summary(progress_pid) do
    Agent.get(progress_pid, fn state ->
      %{status: state.status, processed: state.processed, total: state.total, errors: Enum.reverse(state.errors)}
    end)
  end
end
```

Instead of one module owning the progress protocol, several modules directly manipulate the same `Agent`. This makes the state transitions harder to audit and change safely.
