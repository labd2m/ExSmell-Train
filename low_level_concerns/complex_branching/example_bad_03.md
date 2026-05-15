# Example 3 — Complex branching

This example intentionally demonstrates the **Complex branching** smell.
The function `fetch_invoice/1` handles too many response variations from the same API endpoint in a single `case`, increasing cyclomatic complexity and making the function harder to maintain and test.

```elixir
defmodule BillingClient do
  @moduledoc false

  def fetch_invoice(invoice_id) do
    case get("/api/invoices/#{invoice_id}") do
      {:ok, %Tesla.Env{status: 200, body: %{"paid" => true} = body}} ->
        {:ok, Map.put(body, "state", "settled")}

      {:ok, %Tesla.Env{status: 200, body: %{"paid" => false, "due_date" => due_date} = body}} ->
        {:ok, Map.merge(body, %{"state" => "open", "due_date" => due_date})}

      {:ok, %Tesla.Env{status: 202, body: %{"job_id" => job_id}}} ->
        {:retry_later, job_id}

      {:ok, %Tesla.Env{status: 204}} ->
        {:error, :empty_response}

      {:ok, %Tesla.Env{status: 400, body: %{"error" => "invalid_invoice_id"}}} ->
        {:error, :invalid_invoice_id}

      {:ok, %Tesla.Env{status: 401}} ->
        {:error, :unauthorized}

      {:ok, %Tesla.Env{status: 403, body: %{"reason" => reason}}} ->
        {:error, {:forbidden, reason}}

      {:ok, %Tesla.Env{status: 404}} ->
        {:error, :not_found}

      {:ok, %Tesla.Env{status: 409, body: %{"conflict" => "archived"}}} ->
        {:error, :invoice_archived}

      {:ok, %Tesla.Env{status: 422, body: %{"errors" => errors}}} when is_list(errors) ->
        {:error, {:validation_failed, errors}}

      {:ok, %Tesla.Env{status: 429, headers: headers}} ->
        retry_after = List.keyfind(headers, "retry-after", 0, {"retry-after", "60"}) |> elem(1)
        {:error, {:rate_limited, retry_after}}

      {:ok, %Tesla.Env{status: status, body: body}} when status >= 500 ->
        {:error, {:server_error, status, body}}

      {:error, %Mint.TransportError{reason: :timeout}} ->
        {:error, :timeout}

      {:error, %Mint.TransportError{reason: :econnrefused}} ->
        {:error, :connection_refused}

      {:error, reason} ->
        {:error, {:unexpected_error, reason}}
    end
  end

  defp get(_path) do
    # external HTTP call
    raise "stub"
  end
end
```

Why this is a smell:

- `fetch_invoice/1` centralizes every possible success and failure path in one function.
- A new API response shape forces more branches into the same function.
- The function mixes transport concerns, HTTP status interpretation, and business rules.
- It becomes harder to read, test, and safely modify.
