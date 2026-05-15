smell_id:dynamic_atom_creation

# Example 3

```elixir
defmodule MyApp.EventRouter do
  def route(event_name, payload) do
    event = String.to_atom(event_name)

    apply(MyApp.EventHandlers, event, [payload])
  end
end

defmodule MyApp.EventHandlers do
  def user_created(payload), do: {:ok, payload}
  def user_deleted(payload), do: {:ok, payload}
end
```

Every unseen event name becomes a permanent atom before dispatching.
