smell_id:complex_branching

# Example 01 — Customer API client with many response branches

```elixir
defmodule MyApp.Customers do
  alias Tesla.Env
  import Tesla, only: [get: 1]

  def fetch_customer(customer_id) do
    case get("/customers/#{customer_id}") do
      {:ok, %Env{status: 200, body: %{"data" => customer}}} ->
        {:ok, customer}

      {:ok, %Env{status: 200, body: body}} ->
        {:error, {:unexpected_body, body}}

      {:ok, %Env{status: 400, body: %{"error" => error}}} ->
        {:error, {:bad_request, error}}

      {:ok, %Env{status: 401}} ->
        {:error, :unauthorized}

      {:ok, %Env{status: 403}} ->
        {:error, :forbidden}

      {:ok, %Env{status: 404}} ->
        {:error, :not_found}

      {:ok, %Env{status: 408}} ->
        {:error, :timeout}

      {:ok, %Env{status: 409, body: body}} ->
        {:error, {:conflict, body}}

      {:ok, %Env{status: 422, body: body}} ->
        {:error, {:validation_failed, body}}

      {:ok, %Env{status: 429}} ->
        {:error, :rate_limited}

      {:ok, %Env{status: 500}} ->
        {:error, :server_error}

      {:ok, %Env{status: 503}} ->
        {:error, :service_unavailable}

      {:ok, %Env{status: status, body: body}} ->
        {:error, {:unknown_status, status, body}}

      {:error, :timeout} ->
        {:error, :timeout}

      {:error, :econnrefused} ->
        {:error, :connection_refused}

      {:error, reason} ->
        {:error, {:transport_error, reason}}
    end
  end
end
```

This is a bad example because `fetch_customer/1` concentrates all response handling for a single endpoint in one large `case`, increasing cyclomatic complexity and making the function fragile.
