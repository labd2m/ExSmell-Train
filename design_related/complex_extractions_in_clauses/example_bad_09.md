smell_id:complex_extractions_in_clauses

# Example 09 - Payout processing with extraction overload

```elixir
defmodule Payroll.Payout do
  defstruct [:employee_id, :employee_name, :country, :amount, :currency, :bank_status]
end

defmodule Payroll.Processor do
  alias Payroll.Payout

  def deliver(%Payout{
        employee_id: employee_id,
        employee_name: employee_name,
        country: country,
        amount: amount,
        currency: currency,
        bank_status: bank_status
      })
      when amount > 0 and bank_status == :verified and country in ["BR", "MX", "AR"] do
    {:send, "pay #{employee_name} (#{employee_id}) #{amount} #{currency} in #{country}"}
  end

  def deliver(%Payout{
        employee_id: employee_id,
        employee_name: employee_name,
        country: country,
        amount: amount,
        currency: currency,
        bank_status: bank_status
      })
      when amount > 0 and bank_status == :pending do
    {:hold, "hold payout for #{employee_name} (#{employee_id}) in #{country}; amount=#{amount} #{currency}"}
  end

  def deliver(%Payout{
        employee_id: employee_id,
        employee_name: employee_name,
        country: country,
        amount: amount,
        currency: currency,
        bank_status: bank_status
      })
      when amount <= 0 or bank_status == :rejected do
    {:error, "invalid payout for #{employee_name} (#{employee_id}); amount=#{amount}; country=#{country}; currency=#{currency}; bank_status=#{bank_status}"}
  end
end
```

The repeated extraction makes the function harder to read, especially because the clause head mixes routing logic with values only needed for the returned message.
