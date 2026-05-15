smell_id:speculative_generality

# Example 05

```elixir
defmodule UserSearch do
  def find_by_email(users, email, strategy \ :exact) do
    case strategy do
      :exact -> Enum.find(users, &(&1.email == email))
      :fuzzy -> Enum.find(users, &(&1.email == email))
    end
  end
end
```

The `strategy` parameter was added to support future search modes, but today both branches are identical.
