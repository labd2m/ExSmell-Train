smell_id:data_manipulation_by_migration

# Example 03

```elixir
defmodule Billing.Repo.Migrations.AddAmountCentsToInvoices do
  use Ecto.Migration

  import Ecto.Query
  alias Billing.Invoices.Invoice
  alias Billing.Repo

  def change do
    alter table(:invoices) do
      add :amount_cents, :integer, default: 0, null: false
    end

    from(i in Invoice, select: %{id: i.id, amount: i.amount})
    |> Repo.all()
    |> Enum.each(fn %{id: id, amount: amount} ->
      cents = round(amount * 100)

      from(i in Invoice, where: i.id == ^id)
      |> Repo.update_all(set: [amount_cents: cents])
    end)
  end
end
```

Instead of only evolving the table definition, the migration also converts old monetary values.
