smell_id:speculative_generality

# Example 02

```elixir
defmodule Mailer do
  def send_welcome(user, provider \ :smtp) do
    case provider do
      :smtp -> deliver_with_smtp(user)
      :sendgrid -> deliver_with_smtp(user)
      :ses -> deliver_with_smtp(user)
    end
  end

  defp deliver_with_smtp(user) do
    IO.puts("Sending welcome email to #{user.email}")
  end
end
```

The optional `provider` parameter suggests future pluggability, but every option does the exact same thing.
