smell_id:accessing_non_existent_map_struct_fields

# Example 03 - Metrics aggregation with optional-looking keys

```elixir
defmodule Metrics do
  def summarize(counters) do
    %{
      requests: counters[:requests] || 0,
      failures: counters[:failures] || 0,
      retries: counters[:retries] || 0
    }
  end
end

# Use examples
# iex> Metrics.summarize(%{requests: 100, failures: 2})
# %{requests: 100, failures: 2, retries: 0}
#
# iex> Metrics.summarize(%{requests: 100, failures: 2, retries: nil})
# %{requests: 100, failures: 2, retries: 0}
```

Because `counters[:retries]` returns `nil` for both a missing key and a present key set to `nil`, the function silently hides malformed data.
