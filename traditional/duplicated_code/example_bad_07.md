smell_id:duplicated_code

# Example 07

```elixir
defmodule API.Response do
  def get_customer(id) do
    case HTTP.get("/customers/#{id}") do
      {:ok, %{status: 200, body: body}} -> {:ok, body}
      {:ok, %{status: 404}} -> {:error, :not_found}
      {:ok, %{status: 401}} -> {:error, :unauthorized}
      {:error, reason} -> {:error, reason}
    end
  end

  def get_order(id) do
    case HTTP.get("/orders/#{id}") do
      {:ok, %{status: 200, body: body}} -> {:ok, body}
      {:ok, %{status: 404}} -> {:error, :not_found}
      {:ok, %{status: 401}} -> {:error, :unauthorized}
      {:error, reason} -> {:error, reason}
    end
  end

  def get_invoice(id) do
    case HTTP.get("/invoices/#{id}") do
      {:ok, %{status: 200, body: body}} -> {:ok, body}
      {:ok, %{status: 404}} -> {:error, :not_found}
      {:ok, %{status: 401}} -> {:error, :unauthorized}
      {:error, reason} -> {:error, reason}
    end
  end
end

defmodule HTTP do
  def get(path), do: {:ok, %{status: 200, body: %{path: path}}}
end
```
