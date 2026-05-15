smell_id:unrelated_multi_clause_function

# Example 07

The same function name `sync/1` handles **warehouse stock sync**, **CRM contact sync**, and **feature flag propagation**.

```elixir
defmodule MyApp.Sync do
  defmodule Stock do
    defstruct [:sku, :count]
  end

  defmodule Contact do
    defstruct [:email, :name]
  end

  defmodule Flag do
    defstruct [:name, :enabled]
  end

  @doc """
  Synchronize data.
  """
  def sync(%Stock{sku: sku, count: count}) when is_binary(sku) and is_integer(count) do
    {:warehouse, %{sku: sku, count: count, synced: true}}
  end

  # unrelated: CRM integration logic
  def sync(%Contact{email: email, name: name}) when is_binary(email) and is_binary(name) do
    {:crm, %{email: email, remote_id: "contact-123"}}
  end

  # unrelated: rollout/config logic
  def sync(%Flag{name: name, enabled: enabled}) when is_binary(name) and is_boolean(enabled) do
    {:flags, %{name: name, propagated: true, enabled: enabled}}
  end
end
```
