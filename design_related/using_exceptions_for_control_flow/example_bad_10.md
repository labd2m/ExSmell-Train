smell_id:using_exceptions_for_control_flow

# Example 10

A workflow step lookup raises when a transition is invalid, so the caller must rescue to choose a user-facing error result.

```elixir
defmodule Workflow do
  def next_step!("draft", "submit"), do: "pending_review"
  def next_step!("pending_review", "approve"), do: "published"
  def next_step!("pending_review", "reject"), do: "draft"

  def next_step!(_state, _action) do
    raise RuntimeError, "invalid transition"
  end
end

defmodule ArticleService do
  def move(article, action) do
    try do
      new_state = Workflow.next_step!(article.state, action)
      {:ok, %{article | state: new_state}}
    rescue
      e in RuntimeError ->
        {:error, %{message: e.message, state: article.state}}
    end
  end
end

# Use examples
ArticleService.move(%{id: 1, state: "draft"}, "submit")
ArticleService.move(%{id: 1, state: "draft"}, "approve")
```
