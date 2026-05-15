smell_id:comments

# Example 10

```elixir
defmodule FeatureFlags do
  # Checks whether a feature is enabled for an account.
  # The account can override the global default through its own map.
  # Returns false when the feature is not present anywhere.
  def enabled?(global_flags, account_flags, feature_name) do
    case Map.fetch(account_flags, feature_name) do
      {:ok, value} -> value
      :error -> Map.get(global_flags, feature_name, false)
    end
  end
end
```
