smell_id:long_function

```elixir
defmodule OrderPrinter do
  def print(order, company, io_device) do
    IO.write(io_device, "*** ORDER RECEIPT ***\n")
    IO.write(io_device, "Company: #{company.name}\n")
    IO.write(io_device, "Tax ID: #{company.tax_id}\n")
    IO.write(io_device, "Order ##{order.number}\n")
    IO.write(io_device, "Customer: #{order.customer_name}\n")

    subtotal =
      order.items
      |> Enum.reduce(0, fn item, acc ->
        line_total = item.price * item.quantity
        IO.write(io_device, "#{item.name} x#{item.quantity} - #{line_total}\n")
        acc + line_total
      end)

    discount = if order.coupon, do: subtotal * 0.1, else: 0
    shipping = if subtotal > 100, do: 0, else: 12
    tax = (subtotal - discount) * 0.08
    grand_total = subtotal - discount + shipping + tax

    IO.write(io_device, "Subtotal: #{subtotal}\n")
    IO.write(io_device, "Discount: #{discount}\n")
    IO.write(io_device, "Shipping: #{shipping}\n")
    IO.write(io_device, "Tax: #{tax}\n")
    IO.write(io_device, "Total: #{grand_total}\n")
    IO.write(io_device, "*********************\n")

    :ok
  end
end
```
