smell_id:divergent_change

# Example 03

```elixir
defmodule PM.Project do
  @moduledoc """
  One module changes for workflow rules, reporting rules,
  and third-party webhook processing.
  """

  # Reason to change: workflow policies
  def move_task(task, new_status) do
    allowed = %{todo: [:doing], doing: [:review, :todo], review: [:done, :doing]}

    if new_status in Map.get(allowed, task.status, []) do
      {:ok, %{task | status: new_status}}
    else
      {:error, :invalid_transition}
    end
  end

  # Reason to change: burndown report calculation
  def weekly_velocity(tasks) do
    tasks
    |> Enum.filter(&(&1.status == :done))
    |> Enum.map(& &1.points)
    |> Enum.sum()
  end

  # Reason to change: GitHub webhook handling
  def handle_github_webhook(%{"action" => "opened", "issue" => issue}) do
    {:ok, %{external_id: issue["id"], title: issue["title"]}}
  end

  def handle_github_webhook(_payload) do
    {:error, :ignored}
  end
end
```
