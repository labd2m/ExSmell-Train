smell_id:unrelated_multi_clause_function

# Example 04

A controller helper named `handle/1` mixes **login submission**, **PDF export**, and **webhook verification** in one multi-clause API.

```elixir
defmodule MyAppWeb.MixedControllerLogic do
  def handle(%{"action" => "login", "email" => email, "password" => password})
      when is_binary(email) and is_binary(password) do
    {:ok, %{user_email: email, session_created: true}}
  end

  # unrelated: reporting/export logic
  def handle(%{"action" => "export_pdf", "report_id" => report_id}) when is_binary(report_id) do
    {:ok, %{report_id: report_id, file: "/tmp/#{report_id}.pdf"}}
  end

  # unrelated: webhook security logic
  def handle(%{"action" => "verify_webhook", "signature" => sig, "payload" => payload})
      when is_binary(sig) and is_binary(payload) do
    if byte_size(sig) > 10, do: :verified, else: :invalid_signature
  end
end
```
