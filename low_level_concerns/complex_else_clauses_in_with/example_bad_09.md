# Example 9 of **Complex else clauses in with**

```elixir
defmodule BillingImporter do
  def import_invoice(token, invoice_id) do
    with {:ok, user} <- authorize(token),
         {:ok, %{"status" => 200, "body" => body}} <- fetch_invoice(invoice_id, user),
         {:ok, decoded} <- Base.decode64(body),
         {:ok, invoice} <- Jason.decode(decoded),
         {:ok, normalized} <- validate_invoice(invoice),
         {:ok, record} <- persist_invoice(normalized) do
      {:ok, record}
    else
      {:error, :invalid_token} ->
        {:error, "authentication failed"}

      {:error, :expired_token} ->
        {:error, "authentication failed"}

      {:error, :forbidden} ->
        {:error, "user cannot import invoices"}

      {:error, :timeout} ->
        {:error, "billing service timeout"}

      {:error, :not_found} ->
        {:error, "invoice not found"}

      {:error, :invalid_base64} ->
        {:error, "invoice payload is corrupted"}

      {:error, %Jason.DecodeError{}} ->
        {:error, "invoice payload is not valid json"}

      {:error, {:missing_field, field}} ->
        {:error, "missing required field: #{field}"}

      {:error, {:invalid_field, field}} ->
        {:error, "invalid value for field: #{field}"}

      {:error, :db_unavailable} ->
        {:error, "database unavailable"}

      {:error, :duplicate_invoice} ->
        {:error, "invoice already imported"}

      other ->
        {:error, {:unexpected_failure, other}}
    end
  end

  defp authorize("valid-token"), do: {:ok, %{id: 1, role: :accountant}}
  defp authorize("expired-token"), do: {:error, :expired_token}
  defp authorize(_), do: {:error, :invalid_token}

  defp fetch_invoice("404", _user), do: {:error, :not_found}
  defp fetch_invoice("timeout", _user), do: {:error, :timeout}

  defp fetch_invoice(_invoice_id, %{role: :accountant}) do
    payload =
      %{number: "INV-100", total: 1200, customer_id: 7}
      |> Jason.encode!()
      |> Base.encode64()

    {:ok, %{"status" => 200, "body" => payload}}
  end

  defp fetch_invoice(_invoice_id, _user), do: {:error, :forbidden}

  defp validate_invoice(%{"number" => number, "total" => total, "customer_id" => customer_id})
       when is_binary(number) and is_number(total) and is_integer(customer_id) do
    {:ok, %{number: number, total: total, customer_id: customer_id}}
  end

  defp validate_invoice(%{"number" => _number, "total" => _total}) do
    {:error, {:missing_field, :customer_id}}
  end

  defp validate_invoice(%{"number" => number}) when not is_binary(number) do
    {:error, {:invalid_field, :number}}
  end

  defp validate_invoice(_), do: {:error, {:invalid_field, :payload}}

  defp persist_invoice(%{number: "INV-100"} = invoice), do: {:ok, Map.put(invoice, :id, 99)}
  defp persist_invoice(%{number: "INV-DUP"}), do: {:error, :duplicate_invoice}
  defp persist_invoice(_), do: {:error, :db_unavailable}
end
```

## Why this is a smell

The `with` expression chains several operations, but every failure path is flattened into one large `else` block.  
That makes it harder to understand **which step produced which error**, especially because multiple clauses share similar shapes like `{:error, reason}`.

As the flow grows, this style becomes harder to read, test, and safely modify.
