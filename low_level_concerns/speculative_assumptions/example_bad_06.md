smell_id:speculative_assumptions

# Example 06 - Topic parser for a message bus key

```elixir
defmodule EventTopic do
  @doc """
  Extracts the entity id from a topic formatted as `entity:id:action`.
  """
  def entity_id(topic) do
    topic
    |> String.split(":")
    |> Enum.at(1)
  end
end

# Planned input
EventTopic.entity_id("order:123:created")
# => "123"

# Unplanned input with a richer topic shape gives a misleading value
EventTopic.entity_id("order:region:123:created")
# => "region"
```
