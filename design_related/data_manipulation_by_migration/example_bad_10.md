smell_id:data_manipulation_by_migration

# Example 10

```elixir
defmodule Crm.Repo.Migrations.SplitFullNameOnContacts do
  use Ecto.Migration

  import Ecto.Query
  alias Crm.Contacts.Contact
  alias Crm.Repo

  def up do
    alter table(:contacts) do
      add :first_name, :string
      add :last_name, :string
    end

    from(c in Contact, select: %{id: c.id, full_name: c.full_name})
    |> Repo.all()
    |> Enum.each(fn %{id: id, full_name: full_name} ->
      [first_name | rest] = String.split(full_name, " ")
      last_name = Enum.join(rest, " ")

      from(c in Contact, where: c.id == ^id)
      |> Repo.update_all(set: [first_name: first_name, last_name: last_name])
    end)

    alter table(:contacts) do
      remove :full_name
    end
  end

  def down do
    alter table(:contacts) do
      add :full_name, :string
    end

    from(c in Contact, select: %{id: c.id, first_name: c.first_name, last_name: c.last_name})
    |> Repo.all()
    |> Enum.each(fn %{id: id, first_name: first_name, last_name: last_name} ->
      full_name = Enum.join([first_name, last_name], " ")

      from(c in Contact, where: c.id == ^id)
      |> Repo.update_all(set: [full_name: full_name])
    end)

    alter table(:contacts) do
      remove :first_name
      remove :last_name
    end
  end
end
```

This module mixes column evolution with a bidirectional data transformation for every existing contact.
