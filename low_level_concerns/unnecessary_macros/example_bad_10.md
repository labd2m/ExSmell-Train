smell_id:unnecessary_macros

# Example 10

This macro wraps a simple authorization check. Because the behavior is straightforward and runtime-based, a function would be easier to maintain.

```elixir
defmodule Security do
  defmacro admin?(user) do
    quote do
      unquote(user).role == :admin
    end
  end
end

defmodule AdminController do
  require Security

  def delete_user(current_user, target_user) do
    if Security.admin?(current_user) do
      {:ok, target_user.id}
    else
      {:error, :forbidden}
    end
  end
end
```
