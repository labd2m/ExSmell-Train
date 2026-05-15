smell_id:unsupervised_process

# Example 04 - Registry implemented as many unsupervised Agent processes

```elixir
defmodule Presence.UserState do
  def start(user_id) do
    Agent.start(fn -> %{user_id: user_id, online?: true, last_seen_at: nil} end)
  end

  def touch(pid) do
    Agent.update(pid, fn state -> %{state | last_seen_at: System.system_time(:second)} end)
  end
end

defmodule Presence.Registry do
  def connect_all(user_ids) do
    Enum.into(user_ids, %{}, fn user_id ->
      # One long-lived Agent per user, all created outside supervision.
      {:ok, pid} = Presence.UserState.start(user_id)
      {user_id, pid}
    end)
  end
end
```
