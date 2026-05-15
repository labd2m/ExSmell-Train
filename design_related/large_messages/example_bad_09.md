smell_id:large_messages
example_id:example_bad_09

# Example 09 - Game coordinator sends full world state every tick

```elixir
defmodule PhysicsWorker do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:tick, world_state} ->
        map_size(world_state.players)
        loop()
    end
  end
end

defmodule GameCoordinator do
  def tick(worker_pid) do
    world_state = %{
      players:
        Enum.into(1..200_000, %{}, fn i ->
          {i, %{x: i, y: i * 2, inventory: Enum.to_list(1..30)}}
        end),
      npcs:
        Enum.into(1..100_000, %{}, fn i ->
          {i, %{kind: :merchant, mood: :idle}}
        end)
    }

    # Bad smell: every tick copies the full world state to another process
    send(worker_pid, {:tick, world_state})
  end
end

# Use example:
worker = PhysicsWorker.start()
GameCoordinator.tick(worker)
```

The coordinator sends an enormous state structure on each tick instead of sending only needed slices or commands.
