smell_id:genserver_envy

# Example 5 - Agent used as a feature-flag service

```elixir
defmodule FeatureFlags do
  def start_link() do
    Agent.start_link(fn -> %{flags: %{}, source: "flags.json"} end, name: __MODULE__)
  end

  def enabled?(flag_name, account_id) do
    Agent.get(__MODULE__, fn state ->
      case Map.fetch(state.flags, flag_name) do
        {:ok, %{accounts: accounts}} -> account_id in accounts
        {:ok, true} -> true
        :error -> false
      end
    end)
  end

  def put_flag(flag_name, value) do
    Agent.update(__MODULE__, fn state ->
      %{state | flags: Map.put(state.flags, flag_name, value)}
    end)
  end

  def reload() do
    Agent.update(__MODULE__, fn state ->
      decoded =
        state.source
        |> File.read!()
        |> Jason.decode!()

      normalized =
        for {key, value} <- decoded, into: %{} do
          {String.to_atom(key), value}
        end

      %{state | flags: normalized}
    end)
  end
end
```

This `Agent` is no longer just an accessor for shared state. It performs reload logic, parsing, normalization, and decision-making like a service process.
