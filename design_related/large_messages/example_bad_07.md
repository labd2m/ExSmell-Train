smell_id:large_messages
example_id:example_bad_07

# Example 07 - Metrics worker receives all raw samples at once

```elixir
defmodule DashboardWorker do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:render_metrics, samples} ->
        length(samples)
        loop()
    end
  end
end

defmodule MetricsCollector do
  def push_dashboard_snapshot(worker_pid) do
    samples =
      Enum.map(1..900_000, fn i ->
        %{cpu: rem(i, 100), mem: rem(i, 64), ts: System.system_time(:millisecond)}
      end)

    # Bad smell: huge sample collection copied into another mailbox
    send(worker_pid, {:render_metrics, samples})
  end
end

# Use example:
worker = DashboardWorker.start()
MetricsCollector.push_dashboard_snapshot(worker)
```

A massive list of raw measurements is sent as a message instead of using aggregation or shared storage.
