smell_id:data_manipulation_by_migration

# Example 08

```elixir
defmodule Warehouse.Repo.Migrations.AddDiscontinuedToProducts do
  use Ecto.Migration

  import Ecto.Query
  alias Warehouse.Catalog.Product
  alias Warehouse.Repo

  def up do
    alter table(:products) do
      add :discontinued, :boolean, default: false, null: false
    end

    create index(:products, [:discontinued])

    from(p in Product, where: p.stock == 0 and p.updated_at < ago(365, "day"))
    |> Repo.update_all(set: [discontinued: true])

    from(p in Product, where: p.discontinued == true and p.stock == 0 and is_nil(p.sku))
    |> Repo.delete_all()
  end

  def down do
    drop index(:products, [:discontinued])

    alter table(:products) do
      remove :discontinued
    end
  end
end
```

This migration performs schema changes, backfills a flag, and even deletes data as part of the same evolution step.
