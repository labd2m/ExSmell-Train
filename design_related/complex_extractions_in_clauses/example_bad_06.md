smell_id:complex_extractions_in_clauses

# Example 06 - Invoice classification with large clause heads

```elixir
defmodule Finance.Invoice do
  defstruct [:number, :status, :currency, :total, :customer_name, :due_days]
end

defmodule Finance.Inbox do
  alias Finance.Invoice

  def tag(%Invoice{
        number: number,
        status: status,
        currency: currency,
        total: total,
        customer_name: customer_name,
        due_days: due_days
      })
      when status == :open and due_days < 0 and total > 100_000 do
    {:urgent, "#{customer_name}'s invoice #{number} is overdue in #{currency} for #{total}"}
  end

  def tag(%Invoice{
        number: number,
        status: status,
        currency: currency,
        total: total,
        customer_name: customer_name,
        due_days: due_days
      })
      when status == :open and due_days >= 0 and total > 100_000 do
    {:watch, "#{customer_name}'s invoice #{number} is large, not overdue yet (#{due_days} days)"}
  end

  def tag(%Invoice{
        number: number,
        status: status,
        currency: currency,
        total: total,
        customer_name: customer_name,
        due_days: due_days
      })
      when status in [:paid, :cancelled] do
    {:archive, "archive invoice #{number} for #{customer_name}; total=#{total} #{currency}; due_days=#{due_days}"}
  end
end
```

The function head is cluttered with extracted values for interpolation and logging, mixed with values that actually drive the clause selection.
