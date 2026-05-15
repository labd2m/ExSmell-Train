smell_id:unrelated_multi_clause_function

# Example 09

The function `deliver/1` handles **email sending**, **Slack notifications**, and **database cleanup commands**, which should not share one abstraction.

```elixir
defmodule MyApp.Dispatcher do
  defmodule Email do
    defstruct [:to, :subject]
  end

  defmodule SlackMessage do
    defstruct [:channel, :text]
  end

  defmodule Cleanup do
    defstruct [:table, :days_old]
  end

  @doc """
  Deliver something.
  """
  def deliver(%Email{to: to, subject: subject}) when is_binary(to) and is_binary(subject) do
    {:email_sent, to, subject}
  end

  # unrelated: chat/ops communication
  def deliver(%SlackMessage{channel: channel, text: text}) when is_binary(channel) and is_binary(text) do
    {:slack_sent, channel, text}
  end

  # unrelated: maintenance task
  def deliver(%Cleanup{table: table, days_old: days_old}) when is_binary(table) and is_integer(days_old) do
    {:cleanup_started, table, days_old}
  end
end
```
