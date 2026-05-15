smell_id:unrelated_multi_clause_function

# Example 01

This example mixes **invoice charging**, **refund approval**, and **trial activation** into the same multi-clause function `process/1`.

```elixir
defmodule MyApp.Billing do
  defmodule Invoice do
    defstruct [:id, :amount_cents, :status]
  end

  defmodule Refund do
    defstruct [:id, :reason, :approved?]
  end

  defmodule Trial do
    defstruct [:user_id, :days]
  end

  @doc """
  Process billing-related data.
  """
  def process(%Invoice{status: :pending, amount_cents: cents} = invoice) when cents > 0 do
    %{invoice | status: :charged}
  end

  # unrelated: refund workflow
  def process(%Refund{reason: "duplicate", approved?: false} = refund) do
    %{refund | approved?: true}
  end

  # unrelated: user onboarding/trial activation
  def process(%Trial{user_id: user_id, days: days}) when days in 7..30 do
    %{user_id: user_id, trial_active: true, expires_in_days: days}
  end
end
```
