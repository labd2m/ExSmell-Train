smell_id:agent_obsession

# Example 06 — Game session state mutated by several gameplay modules

```elixir
defmodule Game.SessionState do
  def start_link(player_name) do
    Agent.start_link(fn ->
      %{player: player_name, hp: 100, inventory: [], quests: %{}, zone: "start"}
    end)
  end
end

defmodule Game.Combat do
  def take_damage(session_pid, amount) do
    Agent.update(session_pid, fn state ->
      %{state | hp: max(state.hp - amount, 0)}
    end)
  end
end

defmodule Game.Inventory do
  def add_item(session_pid, item) do
    Agent.update(session_pid, fn state ->
      %{state | inventory: [item | state.inventory]}
    end)
  end
end

defmodule Game.Quests do
  def complete(session_pid, quest_id) do
    Agent.update(session_pid, fn state ->
      put_in(state, [:quests, quest_id], :done)
    end)
  end
end

defmodule Game.Navigation do
  def move_to(session_pid, zone) do
    Agent.update(session_pid, fn state ->
      %{state | zone: zone}
    end)
  end
end

defmodule Game.Debug do
  def state(session_pid) do
    Agent.get(session_pid, & &1)
  end
end
```

The session is treated like a general-purpose server, but the direct `Agent` calls are scattered across many modules. That is a sign the process should have a dedicated API rather than exposed state access everywhere.
