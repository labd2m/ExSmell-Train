smell_id:using_exceptions_for_control_flow

# Example 05

A configuration accessor raises when a key is absent, and boot code rescues to choose a default environment-specific value.

```elixir
defmodule RequiredConfig do
  def fetch!(config, key) do
    case Map.fetch(config, key) do
      {:ok, value} -> value
      :error -> raise KeyError, message: "missing config #{inspect(key)}"
    end
  end
end

defmodule MailerBoot do
  def smtp_port(config) do
    try do
      RequiredConfig.fetch!(config, :smtp_port)
    rescue
      e in KeyError ->
        IO.puts("boot warning: #{Exception.message(e)}")
        2525
    end
  end
end

# Use examples
MailerBoot.smtp_port(%{smtp_port: 587})
MailerBoot.smtp_port(%{})
```
