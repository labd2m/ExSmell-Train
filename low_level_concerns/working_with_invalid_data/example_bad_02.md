smell_id:working_with_invalid_data
# Example 02 — Serializing invalid data with `Jason.encode!/1`

This example forwards arbitrary metadata to `Jason.encode!/1` without checking whether the data is actually JSON-encodable.

## Bad code

```elixir
defmodule MyApp.AuditLogger do
  def log_event(metadata) do
    # expected:
    #   metadata -> map/list/string/number/boolean/nil
    #
    # bad smell:
    #   accepts anything and lets Jason fail later
    Jason.encode!(metadata)
  end
end
```

## Example usage

```elixir
iex> MyApp.AuditLogger.log_event(%{user_id: 1, action: "login"})
"{\"action\":\"login\",\"user_id\":1}"

iex> MyApp.AuditLogger.log_event(%{user_id: 1, socket: self()})
** (Protocol.UndefinedError) protocol Jason.Encoder not implemented for #PID<...>
```

## Why this is a smell

The function should validate or normalize the payload before calling `Jason.encode!/1`.
Instead, the crash appears inside the JSON encoder, far from the place where invalid data first entered the system.
