smell_id:shotgun_surgery

# Shotgun Surgery - Example 03

```elixir
defmodule TaxRate do
  def rate(:food), do: 0.04
  def rate(:electronics), do: 0.18
  # def rate(:books), do: 0.02
end

defmodule TaxLabel do
  def label(:food), do: "Reduced VAT"
  def label(:electronics), do: "Standard VAT"
  # def label(:books), do: "Cultural VAT"
end

defmodule TaxLedger do
  def account(:food), do: "VAT-FOOD"
  def account(:electronics), do: "VAT-ELEC"
  # def account(:books), do: "VAT-BOOKS"
end

defmodule Checkout do
  def tax_line(item) do
    category = item.category

    %{
      label: TaxLabel.label(category),
      account: TaxLedger.account(category),
      value: item.price * TaxRate.rate(category)
    }
  end
end
```
