smell_id:data_manipulation_by_migration

# Example 01

```elixir
defmodule MyApp.Repo.Migrations.AddSlugToUsers do
  use Ecto.Migration

  import Ecto.Query
  alias MyApp.Accounts.User
  alias MyApp.Repo

  def change do
    alter table(:users) do
      add :slug, :string
    end

    create unique_index(:users, [:slug])

    from(u in User, select: %{id: u.id, username: u.username})
    |> Repo.all()
    |> Enum.each(fn %{id: id, username: username} ->
      slug =
        username
        |> String.downcase()
        |> String.replace(" ", "-")

      from(u in User, where: u.id == ^id)
      |> Repo.update_all(set: [slug: slug])
    end)
  end
end
```

This migration changes the schema and also backfills data for all existing users.
