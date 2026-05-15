smell_id:long_function

```elixir
defmodule BillingReport do
  def generate(customer, invoices, discounts, tax_rate) do
    IO.puts("Generating billing report...")

    if is_nil(customer) do
      raise "customer is required"
    end

    full_name = customer.first_name <> " " <> customer.last_name
    IO.puts("Customer: #{full_name}")
    IO.puts("Email: #{customer.email}")

    valid_invoices =
      invoices
      |> Enum.filter(fn invoice -> invoice.status in [:open, :overdue] end)

    subtotal =
      valid_invoices
      |> Enum.reduce(0, fn invoice, acc ->
        line_total =
          invoice.items
          |> Enum.reduce(0, fn item, item_acc ->
            item_acc + item.price * item.quantity
          end)

        IO.puts("Invoice #{invoice.number}: #{line_total}")
        acc + line_total
      end)

    discount_total =
      discounts
      |> Enum.filter(fn d -> d.active end)
      |> Enum.reduce(0, fn d, acc -> acc + d.amount end)

    taxed = (subtotal - discount_total) * tax_rate
    grand_total = subtotal - discount_total + taxed

    IO.puts("Subtotal: #{subtotal}")
    IO.puts("Discounts: #{discount_total}")
    IO.puts("Tax: #{taxed}")
    IO.puts("Grand total: #{grand_total}")

    %{customer: full_name, subtotal: subtotal, discounts: discount_total, tax: taxed, total: grand_total}
  end
end
```
