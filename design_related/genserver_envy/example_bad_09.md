smell_id:genserver_envy

# Example 9 - Agent coordinating retries and remote calls

```elixir
defmodule PaymentRetrier do
  def start_link() do
    Agent.start_link(fn -> %{attempts: %{}} end, name: __MODULE__)
  end

  def charge(invoice_id, payload) do
    Agent.get_and_update(__MODULE__, fn state ->
      current = Map.get(state.attempts, invoice_id, 0) + 1

      case PaymentsAPI.charge(payload) do
        {:ok, response} ->
          {{:ok, response}, %{state | attempts: Map.delete(state.attempts, invoice_id)}}

        {:error, :timeout} when current < 3 ->
          new_attempts = Map.put(state.attempts, invoice_id, current)
          {{:retry, current}, %{state | attempts: new_attempts}}

        {:error, reason} ->
          new_attempts = Map.put(state.attempts, invoice_id, current)
          {{:error, reason}, %{state | attempts: new_attempts}}
      end
    end)
  end
end

defmodule PaymentsAPI do
  def charge(_payload) do
    {:error, :timeout}
  end
end
```

This `Agent` is handling retries, remote-call outcomes, and business policy. It is being used like a command-oriented server, not a simple state holder.
