smell_id:large_messages
example_id:example_bad_05

# Example 05 - Log collector sends a full batch of log lines

```elixir
defmodule LogPersister do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:persist_logs, lines} ->
        Enum.take(lines, 1)
        loop()
    end
  end
end

defmodule LogCollector do
  def flush(buffer, persister_pid) do
    # buffer may hold hundreds of thousands of log lines
    send(persister_pid, {:persist_logs, buffer})
    []
  end
end

# Use example:
buffer =
  Enum.map(1..300_000, fn i ->
    "[info] request=\#{i} path=/checkout status=200 user=\#{i}"
  end)

persister = LogPersister.start()
LogCollector.flush(buffer, persister)
```

The collector sends a huge buffered list directly to another process instead of streaming or chunking it.
