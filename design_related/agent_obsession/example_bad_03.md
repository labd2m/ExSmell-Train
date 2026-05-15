smell_id:agent_obsession

# Example 03 — Chat room participants tracked through direct Agent access

```elixir
defmodule Chat.RoomState do
  def start_link(room_id) do
    Agent.start_link(fn -> %{room_id: room_id, users: [], topic: nil, muted: []} end)
  end
end

defmodule Chat.Joiner do
  def join(room_pid, username) do
    Agent.update(room_pid, fn state ->
      %{state | users: Enum.uniq([username | state.users])}
    end)
  end
end

defmodule Chat.Topic do
  def change(room_pid, topic) do
    Agent.update(room_pid, fn state ->
      Map.put(state, :topic, topic)
    end)
  end
end

defmodule Chat.Moderation do
  def mute(room_pid, username) do
    Agent.update(room_pid, fn state ->
      %{state | muted: Enum.uniq([username | state.muted])}
    end)
  end

  def unmute(room_pid, username) do
    Agent.update(room_pid, fn state ->
      %{state | muted: Enum.reject(state.muted, &(&1 == username))}
    end)
  end
end

defmodule Chat.PresenceView do
  def members(room_pid) do
    Agent.get(room_pid, fn state -> state.users end)
  end

  def room_info(room_pid) do
    Agent.get(room_pid, & &1)
  end
end
```

The room process is effectively being used like a custom server, but the read and write protocol is scattered across unrelated modules rather than encapsulated behind one client API.
