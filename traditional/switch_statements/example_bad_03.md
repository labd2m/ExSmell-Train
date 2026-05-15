smell_id:switch_statements

# Example 03

```elixir
defmodule TaxRules do
  def tax_rate(:book), do: 0.04
  def tax_rate(:food), do: 0.06
  def tax_rate(:electronics), do: 0.18
end

defmodule InvoicePrinter do
  def category_label(:book), do: "Books"
  def category_label(:food), do: "Food"
  def category_label(:electronics), do: "Electronics"
end

defmodule ReturnPolicy do
  def return_days(:book), do: 15
  def return_days(:food), do: 2
  def return_days(:electronics), do: 30
end

defmodule ProductService do
  def describe(product) do
    %{category: category, price: price} = product

    %{
      label: InvoicePrinter.category_label(category),
      tax_rate: TaxRules.tax_rate(category),
      return_days: ReturnPolicy.return_days(category),
      taxed_price: price * (1 + TaxRules.tax_rate(category))
    }
  end
end
```

The same category switching appears in multiple places. Introducing a new category means editing several related condition tables by hand.
