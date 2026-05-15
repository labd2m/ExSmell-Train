smell_id:data_manipulation_by_migration

# Example 04

```elixir
defmodule Cms.Repo.Migrations.CreateTagsAndSeedDefaults do
  use Ecto.Migration

  alias Cms.Repo

  def up do
    create table(:tags) do
      add :name, :string, null: false
      timestamps()
    end

    create unique_index(:tags, [:name])

    Repo.insert_all("tags", [
      %{name: "elixir", inserted_at: NaiveDateTime.utc_now(), updated_at: NaiveDateTime.utc_now()},
      %{name: "phoenix", inserted_at: NaiveDateTime.utc_now(), updated_at: NaiveDateTime.utc_now()},
      %{name: "ecto", inserted_at: NaiveDateTime.utc_now(), updated_at: NaiveDateTime.utc_now()}
    ])
  end

  def down do
    drop table(:tags)
  end
end
```

This migration creates a table and also seeds runtime data inside the same module.
