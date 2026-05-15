smell_id:unrelated_multi_clause_function

# Example 06

A single function `render/1` is abused to perform **HTML rendering**, **invoice total formatting**, and **audit log rendering**.

```elixir
defmodule MyApp.Renderer do
  defmodule Page do
    defstruct [:title, :body]
  end

  defmodule Invoice do
    defstruct [:id, :total_cents]
  end

  defmodule AuditEntry do
    defstruct [:actor, :action, :inserted_at]
  end

  @doc """
  Render output.
  """
  def render(%Page{title: title, body: body}) do
    "<html><h1>#{title}</h1><p>#{body}</p></html>"
  end

  # unrelated: money formatting
  def render(%Invoice{id: id, total_cents: total}) when is_integer(total) do
    "Invoice ##{id}: $#{Float.round(total / 100, 2)}"
  end

  # unrelated: audit/event formatting
  def render(%AuditEntry{actor: actor, action: action, inserted_at: inserted_at}) do
    "[#{inserted_at}] #{actor} performed #{action}"
  end
end
```
