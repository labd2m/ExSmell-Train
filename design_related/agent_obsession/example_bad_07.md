smell_id:agent_obsession

# Example 07 — Rate limiter bucket changed by request, admin, and telemetry modules

```elixir
defmodule RateLimit.Bucket do
  def start_link do
    Agent.start_link(fn -> %{count: 0, reset_at: nil, blocked: false} end, name: __MODULE__)
  end
end

defmodule RateLimit.Requests do
  def hit do
    Agent.update(RateLimit.Bucket, fn state ->
      %{state | count: state.count + 1}
    end)
  end
end

defmodule RateLimit.Admin do
  def block do
    Agent.update(RateLimit.Bucket, fn state ->
      %{state | blocked: true}
    end)
  end

  def unblock do
    Agent.update(RateLimit.Bucket, fn state ->
      %{state | blocked: false}
    end)
  end
end

defmodule RateLimit.Resetter do
  def reset(now) do
    Agent.update(RateLimit.Bucket, fn _state ->
      %{count: 0, reset_at: now, blocked: false}
    end)
  end
end

defmodule RateLimit.Telemetry do
  def snapshot do
    Agent.get(RateLimit.Bucket, fn state -> state end)
  end
end
```

Multiple modules know the internal structure of the bucket state and mutate it directly. This is classic `Agent` obsession: the shared process becomes an unowned bag of state.
