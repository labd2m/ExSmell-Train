smell_id:unrelated_multi_clause_function

# Example 10

This example spreads unrelated behaviors across one `execute/1` function: **payment capture**, **newsletter unsubscribe**, and **audit export**.

```elixir
defmodule MyApp.Executor do
  defmodule CapturePayment do
    defstruct [:payment_id, :amount_cents]
  end

  defmodule Unsubscribe do
    defstruct [:email, :list]
  end

  defmodule ExportAudit do
    defstruct [:from, :to, :format]
  end

  @doc """
  Execute a command.
  """
  def execute(%CapturePayment{payment_id: payment_id, amount_cents: amount})
      when is_binary(payment_id) and is_integer(amount) and amount > 0 do
    {:captured, payment_id, amount}
  end

  # unrelated: marketing subscription logic
  def execute(%Unsubscribe{email: email, list: list}) when is_binary(email) and is_binary(list) do
    {:unsubscribed, email, list}
  end

  # unrelated: compliance/reporting logic
  def execute(%ExportAudit{from: from, to: to, format: format}) when format in [:csv, :json] do
    {:export_ready, %{from: from, to: to, format: format}}
  end
end
```
