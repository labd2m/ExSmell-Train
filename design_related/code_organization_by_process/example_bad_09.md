smell_id:code_organization_by_process

# Example 09 - Report formatting handled by a dedicated process

```elixir
defmodule ReportPresenter do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, :ok)
  end

  def csv_row(pid, fields) do
    GenServer.call(pid, {:csv_row, fields})
  end

  def heading(pid, text) do
    GenServer.call(pid, {:heading, text})
  end

  def init(:ok), do: {:ok, nil}

  def handle_call({:csv_row, fields}, _from, state) do
    {:reply, Enum.join(fields, ","), state}
  end

  def handle_call({:heading, text}, _from, state) do
    {:reply, "=== #{text} ===", state}
  end
end

{:ok, pid} = ReportPresenter.start_link()
ReportPresenter.csv_row(pid, ["Ana", "42", "active"])
ReportPresenter.heading(pid, "Users")
```
