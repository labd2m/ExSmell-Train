smell_id:speculative_generality

# Example 08

```elixir
defmodule AuditLog do
  def record(action, user, metadata \ %{}) do
    %{
      action: action,
      user_id: user.id,
      metadata: metadata
    }
  end

  def verbose_record(action, user, metadata \ %{}) do
    record(action, user, metadata)
  end
end
```

`verbose_record/3` was introduced as a future extension point, but it is just an unused wrapper around `record/3`.
