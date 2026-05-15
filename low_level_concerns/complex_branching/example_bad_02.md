smell_id:complex_branching

# Example 02 — Payment capture with nested branching

```elixir
defmodule MyApp.Payments do
  alias Tesla.Env
  import Tesla, only: [post: 2]

  def capture_payment(payment_id) do
    case post("/payments/#{payment_id}/capture", %{}) do
      {:ok, %Env{status: 200, body: %{"status" => "captured"} = body}} ->
        {:ok, body}

      {:ok, %Env{status: 200, body: %{"status" => "pending"} = body}} ->
        case Map.get(body, "next_action") do
          "retry" -> {:error, :retry_later}
          "3ds" -> {:error, :requires_3ds}
          "review" -> {:error, :manual_review}
          nil -> {:error, :pending}
          other -> {:error, {:unknown_next_action, other}}
        end

      {:ok, %Env{status: 202}} ->
        {:error, :processing}

      {:ok, %Env{status: 400, body: %{"code" => "invalid_amount"}}} ->
        {:error, :invalid_amount}

      {:ok, %Env{status: 400, body: %{"code" => "invalid_currency"}}} ->
        {:error, :invalid_currency}

      {:ok, %Env{status: 401}} ->
        {:error, :bad_credentials}

      {:ok, %Env{status: 402, body: %{"code" => "card_declined"}}} ->
        {:error, :card_declined}

      {:ok, %Env{status: 402, body: %{"code" => "insufficient_funds"}}} ->
        {:error, :insufficient_funds}

      {:ok, %Env{status: 402, body: %{"code" => "expired_card"}}} ->
        {:error, :expired_card}

      {:ok, %Env{status: 409}} ->
        {:error, :already_captured}

      {:ok, %Env{status: 422, body: body}} ->
        {:error, {:validation_errors, body}}

      {:ok, %Env{status: 500}} ->
        {:error, :gateway_failure}

      {:error, :timeout} ->
        {:error, :timeout}

      {:error, reason} ->
        {:error, {:gateway_error, reason}}
    end
  end
end
```

This is a bad example because `capture_payment/1` mixes many status-code branches with another nested `case`, turning one boundary function into a dense decision tree.
