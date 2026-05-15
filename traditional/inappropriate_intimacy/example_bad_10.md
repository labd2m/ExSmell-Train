smell_id:inappropriate_intimacy

# Example 10 - Billing module manipulating account state details

```elixir
defmodule Account do
  defstruct [:balance_cents, :credit_limit_cents, :status, :flags]
end

defmodule Billing do
  def charge(account, amount_cents) do
    blocked = Enum.member?(account.flags, :blocked)

    if account.status == :open and not blocked do
      if account.balance_cents + amount_cents <= account.credit_limit_cents do
        %{account | balance_cents: account.balance_cents + amount_cents}
      else
        {:error, :limit_exceeded}
      end
    else
      {:error, :unavailable_account}
    end
  end
end
```

`Billing.charge/2` knows about the internal fields and invariants of `Account`, increasing coupling and reducing cohesion.
