smell_id:speculative_generality

# Example 07

```elixir
defmodule InvoiceNumber do
  def generate(company_id, prefix \ "INV", separator \ "-") do
    prefix <> separator <> Integer.to_string(company_id)
  end
end

# ...usage examples...

InvoiceNumber.generate(12)
InvoiceNumber.generate(13)
InvoiceNumber.generate(14)
```

The optional `prefix` and `separator` parameters suggest configurability, but all real call sites always use the defaults.
