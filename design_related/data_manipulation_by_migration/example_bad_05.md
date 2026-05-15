smell_id:data_manipulation_by_migration

# Example 05

```elixir
defmodule Projects.Repo.Migrations.AddArchivedAtToProjects do
  use Ecto.Migration

  import Ecto.Query
  alias Projects.Workspace.Project
  alias Projects.Repo

  def up do
    alter table(:projects) do
      add :archived_at, :utc_datetime
    end

    from(p in Project, where: p.is_archived == true)
    |> Repo.update_all(set: [archived_at: DateTime.utc_now()])

    alter table(:projects) do
      remove :is_archived
    end
  end

  def down do
    alter table(:projects) do
      add :is_archived, :boolean, default: false
    end

    from(p in Project, where: not is_nil(p.archived_at))
    |> Repo.update_all(set: [is_archived: true])

    alter table(:projects) do
      remove :archived_at
    end
  end
end
```

The migration both reshapes the schema and migrates business data between columns.
