smell_id:inappropriate_intimacy

# Example 08 - Catalog touching supplier internals

```elixir
defmodule Supplier do
  defstruct [:name, :country, :rating, :preferred_currency]
end

defmodule Catalog do
  def featured_label(%Supplier{} = supplier) do
    country = String.upcase(supplier.country)

    if supplier.rating >= 4.8 do
      "[TOP #{country}] prices in #{supplier.preferred_currency}"
    else
      "[#{country}] prices in #{supplier.preferred_currency}"
    end
  end
end
```

`Catalog.featured_label/1` is tightly coupled to the internal representation of `Supplier`.
