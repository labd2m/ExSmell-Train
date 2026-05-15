smell_id:unsupervised_process

# Example 10 - Library API creates named counters directly and encourages many global processes

```elixir
defmodule Counter do
  use GenServer

  def start(initial_value, pid_name \\ __MODULE__) when is_integer(initial_value) do
    GenServer.start(__MODULE__, initial_value, name: pid_name)
  end

  def get(pid_name \\ __MODULE__) do
    GenServer.call(pid_name, :get)
  end

  def bump(value, pid_name \\ __MODULE__) do
    GenServer.call(pid_name, {:bump, value})
  end

  @impl true
  def init(counter), do: {:ok, counter}

  @impl true
  def handle_call(:get, _from, counter) do
    {:reply, counter, counter}
  end

  @impl true
  def handle_call({:bump, value}, _from, counter) do
    {:reply, counter + value, counter + value}
  end
end

defmodule Reporting do
  def boot_department_counters do
    Enum.each([SalesCounter, SupportCounter, MarketingCounter, OpsCounter], fn name ->
      # Multiple permanent processes are created directly and globally named.
      # They are not started by any supervisor tree.
      {:ok, _pid} = Counter.start(0, name)
    end)
  end
end
```
