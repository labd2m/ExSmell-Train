smell_id:accessing_non_existent_map_struct_fields

# Example 08 - Authorization logic based on possibly absent keys

```elixir
defmodule AccessControl do
  def can_delete?(user) do
    user[:role] == :admin or user[:can_delete] == true
  end
end

# Use examples
# iex> AccessControl.can_delete?(%{role: :admin})
# true
#
# iex> AccessControl.can_delete?(%{})
# false
#
# iex> AccessControl.can_delete?(%{role: nil, can_delete: nil})
# false
```

Dynamic access causes empty maps and explicitly nil authorization data to behave the same, which can hide data-shape problems in permission checks.
