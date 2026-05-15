smell_id:large_messages
example_id:example_bad_04

# Example 04 - Loader sends a giant cache snapshot to a process

```elixir
defmodule CacheServer do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:replace_cache, snapshot} ->
        map_size(snapshot)
        loop()
    end
  end
end

defmodule CacheWarmup do
  def warm(server_pid) do
    snapshot =
      Enum.into(1..800_000, %{}, fn id ->
        {"product_\#{id}", %{price: id, tags: Enum.to_list(1..10)}}
      end)

    # Bad smell: a very large map is copied in a single message
    send(server_pid, {:replace_cache, snapshot})
  end
end

# Use example:
server = CacheServer.start()
CacheWarmup.warm(server)
```

The warmup process constructs an enormous snapshot and transfers the full map through message passing.
