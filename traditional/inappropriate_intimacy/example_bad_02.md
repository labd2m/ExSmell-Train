smell_id:inappropriate_intimacy

# Example 02 - Report module formatting customer internals

```elixir
defmodule Customer do
  defstruct [:first_name, :last_name, :email, :address]
end

defmodule Address do
  defstruct [:street, :city, :zip_code]
end

defmodule CustomerReport do
  def summary(%Customer{} = customer) do
    name = "#{customer.first_name} #{customer.last_name}"
    city = customer.address.city
    zip = customer.address.zip_code

    "#{name} <#{customer.email}> from #{city} (#{zip})"
  end
end
```

The reporting code reaches into nested `Customer` and `Address` internals instead of relying on a cohesive API from those modules.
