smell_id:data_manipulation_by_migration

# Example 09

```elixir
defmodule Hr.Repo.Migrations.AddNormalizedEmailToEmployees do
  use Ecto.Migration

  import Ecto.Query
  alias Hr.People.Employee
  alias Hr.Repo

  def change do
    alter table(:employees) do
      add :normalized_email, :string
    end

    create unique_index(:employees, [:normalized_email])

    from(e in Employee, select: %{id: e.id, email: e.email})
    |> Repo.all()
    |> Enum.each(fn %{id: id, email: email} ->
      normalized =
        email
        |> String.trim()
        |> String.downcase()

      from(e in Employee, where: e.id == ^id)
      |> Repo.update_all(set: [normalized_email: normalized])
    end)
  end
end
```

The migration changes the schema and also contains a full data normalization routine.
