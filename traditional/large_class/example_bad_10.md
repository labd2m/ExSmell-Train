smell_id:large_class

# Example 10

```elixir
defmodule ProjectWorkspace do
  alias PM.Notifier
  alias PM.Storage
  alias PM.Billing

  # Task management
  def create_task(project, attrs), do: Map.put(attrs, :project_id, project.id)
  def complete_task(task), do: %{task | status: :done}
  def reopen_task(task), do: %{task | status: :open}

  # Member management
  def invite_member(project, email), do: %{project_id: project.id, email: email}
  def remove_member(project, member_id), do: {:ok, {project.id, member_id}}

  # File management
  def upload_file(project, file) do
    Storage.put(project.id, file)
  end

  def delete_file(project, file_id) do
    Storage.delete(project.id, file_id)
  end

  # Notifications
  def notify_assignment(user, task) do
    Notifier.send(user.id, "You were assigned to #{task.title}")
  end

  # Billing
  def charge_extra_storage(project, gb) do
    Billing.charge(project.account_id, gb * 2.5)
  end

  # Reporting
  def overdue_task_count(tasks) do
    tasks
    |> Enum.filter(&(&1.status != :done and Date.compare(&1.due_date, Date.utc_today()) == :lt))
    |> Enum.count()
  end
end
```
