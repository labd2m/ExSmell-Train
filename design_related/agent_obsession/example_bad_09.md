smell_id:agent_obsession

# Example 09 — Test support and production code both reach into the same Agent

```elixir
defmodule Mailbox.Store do
  def start_link do
    Agent.start_link(fn -> %{sent: [], failed: []} end, name: __MODULE__)
  end
end

defmodule Mailer do
  def send_email(email) do
    Agent.update(Mailbox.Store, fn state ->
      %{state | sent: [email | state.sent]}
    end)
  end
end

defmodule Mailer.Retry do
  def mark_failed(email) do
    Agent.update(Mailbox.Store, fn state ->
      %{state | failed: [email | state.failed]}
    end)
  end
end

defmodule Mailer.Dashboard do
  def stats do
    Agent.get(Mailbox.Store, fn state ->
      %{sent_count: length(state.sent), failed_count: length(state.failed)}
    end)
  end
end

defmodule MailerTestHelpers do
  def reset_store do
    Agent.update(Mailbox.Store, fn _ ->
      %{sent: [], failed: []}
    end)
  end

  def sent_messages do
    Agent.get(Mailbox.Store, fn state -> state.sent end)
  end
end
```

The process contract is not encapsulated. Production modules and test helpers both directly manipulate the `Agent`, increasing coupling and making refactoring risky.
