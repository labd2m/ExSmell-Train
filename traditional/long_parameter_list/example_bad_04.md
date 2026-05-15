smell_id:long_parameter_list

```elixir
defmodule Reports do
  def generate_sales_report(company_id, from_date, to_date, region, channel, currency, include_refunds, group_by) do
    %{
      company_id: company_id,
      period: {from_date, to_date},
      region: region,
      channel: channel,
      currency: currency,
      include_refunds: include_refunds,
      group_by: group_by
    }
  end
end

Reports.generate_sales_report(
  42,
  ~D[2026-01-01],
  ~D[2026-03-31],
  "LATAM",
  :online,
  "BRL",
  true,
  :week
)
```
