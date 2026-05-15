smell_id:agent_obsession

# Example 08 — Audit log assembled through direct Agent updates across contexts

```elixir
defmodule Audit.Buffer do
  def start_link do
    Agent.start_link(fn -> [] end, name: __MODULE__)
  end
end

defmodule Accounts.Audit do
  def user_created(user_id) do
    Agent.update(Audit.Buffer, fn entries ->
      [%{event: :user_created, user_id: user_id} | entries]
    end)
  end
end

defmodule Billing.Audit do
  def invoice_paid(invoice_id) do
    Agent.update(Audit.Buffer, fn entries ->
      [%{event: :invoice_paid, invoice_id: invoice_id} | entries]
    end)
  end
end

defmodule Security.Audit do
  def login_failed(email) do
    Agent.update(Audit.Buffer, fn entries ->
      [%{event: :login_failed, email: email, at: System.system_time(:second)} | entries]
    end)
  end
end

defmodule Audit.Exporter do
  def flush do
    Agent.get_and_update(Audit.Buffer, fn entries ->
      {Enum.reverse(entries), []}
    end)
  end
end
```

The audit process is shared globally, and each bounded context writes straight to it. Event shape, ordering expectations, and flushing behavior are all coupled to direct `Agent` usage across the codebase.
