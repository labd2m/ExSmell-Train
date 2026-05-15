smell_id:shotgun_surgery

# Shotgun Surgery - Example 05

```elixir
defmodule ReportColumns do
  def columns(:sales), do: [:date, :seller, :total]
  def columns(:refunds), do: [:date, :agent, :reason, :total]
  # def columns(:chargebacks), do: [:date, :bank, :reason, :total]
end

defmodule ReportFilename do
  def filename(:sales), do: "sales.csv"
  def filename(:refunds), do: "refunds.csv"
  # def filename(:chargebacks), do: "chargebacks.csv"
end

defmodule ReportPermissions do
  def role(:sales), do: :manager
  def role(:refunds), do: :support_manager
  # def role(:chargebacks), do: :finance_manager
end

defmodule ReportExport do
  def export(type, rows) do
    %{columns: ReportColumns.columns(type), file: ReportFilename.filename(type), rows: rows}
  end
end
```
