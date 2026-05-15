smell_id:speculative_assumptions

# Example 01 - Query string parsing with speculative extraction

```elixir
defmodule QueryExtractor do
  @doc """
  Returns the value for a query parameter.

  This implementation assumes each segment always has exactly one `=` and that
  the desired value is always in position 1 after splitting.
  """
  def get_param(query_string, desired_key) do
    query_string
    |> String.split("&")
    |> Enum.find_value(fn pair ->
      pieces = String.split(pair, "=")
      Enum.at(pieces, 0) == desired_key && Enum.at(pieces, 1)
    end)
  end
end

# Planned input
QueryExtractor.get_param("page=10&sort=asc&lang=en", "sort")
# => "asc"

# Unplanned input silently returns the wrong value
QueryExtractor.get_param("page=10&sort=direction=asc&lang=en", "sort")
# => "direction"
```
