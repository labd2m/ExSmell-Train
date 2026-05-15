smell_id:dynamic_atom_creation

# Example 2

```elixir
defmodule MyApp.ApiClient do
  def parse_status(%{"status" => status}) do
    String.to_atom(status)
  end
end

defmodule MyApp.Sync do
  def sync(response_body) do
    case MyApp.ApiClient.parse_status(response_body) do
      :ok -> :synced
      :pending -> :retry_later
      :failed -> :stop
      other -> {:unknown_remote_status, other}
    end
  end
end
```

The status field comes from a remote API and is converted dynamically into atoms.
