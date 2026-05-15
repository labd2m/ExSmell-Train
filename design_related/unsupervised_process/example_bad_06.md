smell_id:unsupervised_process

# Example 06 - One GenServer per uploaded file started from a batch importer

```elixir
defmodule Import.FileTracker do
  use GenServer

  def start(file_path) do
    GenServer.start(__MODULE__, %{file_path: file_path, processed: 0})
  end

  def mark_line(pid) do
    GenServer.cast(pid, :mark_line)
  end

  @impl true
  def init(state), do: {:ok, state}

  @impl true
  def handle_cast(:mark_line, state) do
    {:noreply, %{state | processed: state.processed + 1}}
  end
end

defmodule Import.Batch do
  def run(files) do
    Enum.each(files, fn file_path ->
      # Trackers can outlive the import call and accumulate over time,
      # but they are not under any supervisor.
      {:ok, pid} = Import.FileTracker.start(file_path)
      Import.FileTracker.mark_line(pid)
    end)
  end
end
```
