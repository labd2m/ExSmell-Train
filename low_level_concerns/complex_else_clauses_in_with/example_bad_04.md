smell_id:complex_else_clauses_in_with

# Example 04

```elixir
defmodule MyApp.PurchaseImporter do
  alias MyApp.{CSV, Repo, Purchase}

  def import_row(row) do
    with {:ok, columns} <- CSV.parse_line(row),
         {:ok, attrs} <- to_attrs(columns),
         {:ok, changeset} <- Purchase.changeset(%Purchase{}, attrs),
         {:ok, purchase} <- Repo.insert(changeset) do
      {:ok, purchase}
    else
      {:error, :invalid_csv} -> {:error, :bad_format}
      {:error, {:missing_column, column}} -> {:error, {:bad_format, column}}
      {:error, {:invalid_value, field}} -> {:error, {:invalid_input, field}}
      {:error, %Ecto.Changeset{} = changeset} -> {:error, {:validation_failed, changeset}}
      {:error, :timeout} -> {:error, :db_timeout}
      {:error, reason} -> {:error, {:db_error, reason}}
    end
  end

  defp to_attrs([sku, quantity, total]) do
    with {:ok, quantity} <- parse_integer(quantity),
         {:ok, total} <- parse_decimal(total) do
      {:ok, %{sku: sku, quantity: quantity, total: total}}
    else
      :error -> {:error, {:invalid_value, :numeric_field}}
    end
  end

  defp to_attrs(_), do: {:error, {:missing_column, :unknown}}

  defp parse_integer(value) do
    case Integer.parse(value) do
      {number, ""} -> {:ok, number}
      _ -> :error
    end
  end

  defp parse_decimal(value) do
    case Decimal.parse(value) do
      {number, ""} -> {:ok, number}
      _ -> :error
    end
  end
end
```

The main function swallows CSV parsing, conversion, changeset creation, and database insertion.
Its `else` block becomes a funnel for many unrelated failure modes.
