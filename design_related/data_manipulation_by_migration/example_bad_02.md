smell_id:data_manipulation_by_migration

# Example 02

```elixir
defmodule Shop.Repo.Migrations.AddStatusToOrders do
  use Ecto.Migration

  import Ecto.Query
  alias Shop.Sales.Order
  alias Shop.Repo

  def up do
    alter table(:orders) do
      add :status, :string, default: "pending", null: false
    end

    create index(:orders, [:status])

    from(o in Order, where: o.paid_at != nil)
    |> Repo.update_all(set: [status: "paid"])

    from(o in Order, where: o.cancelled_at != nil)
    |> Repo.update_all(set: [status: "cancelled"])
  end

  def down do
    drop index(:orders, [:status])

    alter table(:orders) do
      remove :status
    end
  end
end
```

The migration performs a structural change and then updates existing rows to derive business state.
