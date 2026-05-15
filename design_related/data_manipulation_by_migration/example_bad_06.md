smell_id:data_manipulation_by_migration

# Example 06

```elixir
defmodule Accounts.Repo.Migrations.AddRoleToMemberships do
  use Ecto.Migration

  import Ecto.Query
  alias Accounts.Organizations.Membership
  alias Accounts.Repo

  def change do
    alter table(:memberships) do
      add :role, :string, default: "member", null: false
    end

    create index(:memberships, [:role])

    from(m in Membership, where: m.owner == true)
    |> Repo.update_all(set: [role: "owner"])

    from(m in Membership, where: m.admin == true)
    |> Repo.update_all(set: [role: "admin"])
  end
end
```

A single migration is responsible for both adding the new column and deciding how historical rows should be reclassified.
