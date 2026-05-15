smell_id:agent_obsession

# Example 02 — Feature flags manipulated all over the system

```elixir
defmodule MyApp.FlagStore do
  def start_link do
    Agent.start_link(fn -> %{} end, name: __MODULE__)
  end
end

defmodule MyApp.AdminPanel do
  def enable(flag_name) do
    Agent.update(MyApp.FlagStore, fn flags ->
      Map.put(flags, flag_name, true)
    end)
  end
end

defmodule MyApp.ReleaseTools do
  def disable(flag_name) do
    Agent.update(MyApp.FlagStore, fn flags ->
      Map.put(flags, flag_name, false)
    end)
  end
end

defmodule MyApp.Bootstrap do
  def seed_defaults do
    Agent.update(MyApp.FlagStore, fn flags ->
      Map.merge(flags, %{"new_ui" => false, "payments_v2" => true})
    end)
  end
end

defmodule MyApp.FeatureGate do
  def enabled?(flag_name) do
    Agent.get(MyApp.FlagStore, fn flags ->
      Map.get(flags, flag_name, false)
    end)
  end
end

defmodule MyApp.DebugTools do
  def dump do
    Agent.get(MyApp.FlagStore, & &1)
  end
end
```

Different modules directly change the same `Agent` state. The data format and rules for flag names are uncontrolled because there is no dedicated API around the shared process.
