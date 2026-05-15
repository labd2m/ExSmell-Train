smell_id:code_organization_by_process

# Example 05 - Date helpers behind message passing

```elixir
defmodule DateHelpers do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, :ok)
  end

  def end_of_month(pid, date) do
    GenServer.call(pid, {:end_of_month, date})
  end

  def weekend?(pid, date) do
    GenServer.call(pid, {:weekend?, date})
  end

  def init(:ok), do: {:ok, nil}

  def handle_call({:end_of_month, date}, _from, state) do
    last_day = Date.days_in_month(date)
    {:reply, %Date{date | day: last_day}, state}
  end

  def handle_call({:weekend?, date}, _from, state) do
    {:reply, Date.day_of_week(date) in [6, 7], state}
  end
end

{:ok, pid} = DateHelpers.start_link()
DateHelpers.end_of_month(pid, ~D[2026-04-22])
DateHelpers.weekend?(pid, ~D[2026-04-25])
```
