smell_id:dynamic_atom_creation

# Example 10

```elixir
defmodule MyApp.TelemetryBridge do
  def metric_name(source, action) do
    "#{source}_#{action}"
    |> String.downcase()
    |> String.to_atom()
  end

  def emit(source, action, measurements) do
    name = metric_name(source, action)
    :telemetry.execute([:my_app, name], measurements, %{})
  end
end

# Examples:
# MyApp.TelemetryBridge.emit("Checkout", "Started", %{count: 1})
# MyApp.TelemetryBridge.emit("Tenant-812", "Clicked-Banner-44", %{count: 1})
```

New combinations of `source` and `action` create new atoms indefinitely.
