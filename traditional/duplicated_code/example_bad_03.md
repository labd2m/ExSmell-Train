# Example 3 of **Duplicated Code**

```elixir
defmodule ReportCenter do
  def export_sales_report(user, company_id) do
    if user.role == :admin and user.company_id == company_id do
      {:ok, "sales report exported"}
    else
      {:error, :unauthorized}
    end
  end

  def export_inventory_report(user, company_id) do
    if user.role == :admin and user.company_id == company_id do
      {:ok, "inventory report exported"}
    else
      {:error, :unauthorized}
    end
  end

  def export_financial_report(user, company_id) do
    if user.role == :admin and user.company_id == company_id do
      {:ok, "financial report exported"}
    else
      {:error, :unauthorized}
    end
  end
end

# Use examples

admin = %{id: 1, role: :admin, company_id: 10}
employee = %{id: 2, role: :employee, company_id: 10}

ReportCenter.export_sales_report(admin, 10)
# {:ok, "sales report exported"}

ReportCenter.export_inventory_report(employee, 10)
# {:error, :unauthorized}
```

## Why this is a smell

The same authorization rule is duplicated in multiple functions:

```elixir
if user.role == :admin and user.company_id == company_id do
  ...
else
  {:error, :unauthorized}
end
```

This causes maintenance problems because any future change to the permission rule must be repeated in every function.

For example, if the system later allows managers to export some reports, a developer might update one function and forget the others, creating inconsistent behavior.
