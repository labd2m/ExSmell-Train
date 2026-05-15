smell_id:complex_branching

# Example 04 — Background job processing many API outcomes

```elixir
defmodule MyApp.Workers.SyncInventory do
  use Oban.Worker
  alias Tesla.Env
  import Tesla, only: [get: 1]
  require Logger

  @impl Oban.Worker
  def perform(%Oban.Job{args: %{"sku" => sku}}) do
    case get("/inventory/#{sku}") do
      {:ok, %Env{status: 200, body: %{"quantity" => quantity}}} when quantity > 0 ->
        Logger.info("Inventory updated for #{sku}")
        :ok

      {:ok, %Env{status: 200, body: %{"quantity" => 0}}} ->
        Logger.warning("Out of stock for #{sku}")
        :ok

      {:ok, %Env{status: 200, body: body}} ->
        Logger.error("Unexpected body: #{inspect(body)}")
        {:error, :unexpected_body}

      {:ok, %Env{status: 204}} ->
        Logger.warning("Empty inventory response")
        {:error, :empty_response}

      {:ok, %Env{status: 401}} ->
        Logger.error("Inventory API credentials rejected")
        {:cancel, :unauthorized}

      {:ok, %Env{status: 404}} ->
        Logger.info("SKU #{sku} not found upstream")
        :discard

      {:ok, %Env{status: 409}} ->
        Logger.warning("Conflict while syncing #{sku}")
        {:snooze, 60}

      {:ok, %Env{status: 429}} ->
        Logger.warning("Rate limited while syncing #{sku}")
        {:snooze, 300}

      {:ok, %Env{status: 500}} ->
        Logger.error("Remote inventory server failed")
        {:error, :server_error}

      {:ok, %Env{status: status}} ->
        Logger.error("Unknown status #{status}")
        {:error, {:unknown_status, status}}

      {:error, :timeout} ->
        Logger.warning("Timeout while syncing #{sku}")
        {:snooze, 120}

      {:error, :nxdomain} ->
        Logger.error("DNS error while syncing #{sku}")
        {:error, :dns_failure}

      {:error, reason} ->
        Logger.error("Unexpected transport error: #{inspect(reason)}")
        {:error, reason}
    end
  end
end
```

This is a bad example because the job's `perform/1` function tries to decide logging, retry policy, discard policy, and API interpretation all at once.
