smell_id:complex_else_clauses_in_with

# Example 03

```elixir
defmodule MyApp.ConfigLoader do
  def load_runtime_config(path) do
    with {:ok, raw} <- File.read(path),
         {:ok, json} <- Jason.decode(raw),
         {:ok, endpoint} <- fetch_required(json, "endpoint"),
         {:ok, port} <- fetch_required(json, "port"),
         true <- is_integer(port) do
      {:ok, %{endpoint: endpoint, port: port}}
    else
      {:error, :enoent} -> {:error, :config_not_found}
      {:error, :eacces} -> {:error, :config_forbidden}
      {:error, %Jason.DecodeError{}} -> {:error, :invalid_json}
      {:error, {:missing_key, "endpoint"}} -> {:error, :missing_endpoint}
      {:error, {:missing_key, "port"}} -> {:error, :missing_port}
      false -> {:error, :port_must_be_integer}
    end
  end

  defp fetch_required(map, key) do
    case Map.fetch(map, key) do
      {:ok, value} -> {:ok, value}
      :error -> {:error, {:missing_key, key}}
    end
  end
end
```

The `else` clause must decode where each failure shape came from: file system, JSON parsing,
missing keys, or a boolean guard. That centralizes too many responsibilities.
