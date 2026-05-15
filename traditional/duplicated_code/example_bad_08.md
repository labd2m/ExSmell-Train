smell_id:duplicated_code

# Example 08

```elixir
defmodule Catalog.Product do
  defstruct [:name, :price, :active]
end

defmodule Catalog.Search do
  def by_name(products, name) do
    products
    |> Enum.filter(& &1.active)
    |> Enum.filter(&(String.contains?(String.downcase(&1.name), String.downcase(name))))
    |> Enum.sort_by(& &1.price)
  end

  def by_prefix(products, prefix) do
    products
    |> Enum.filter(& &1.active)
    |> Enum.filter(&(String.starts_with?(String.downcase(&1.name), String.downcase(prefix))))
    |> Enum.sort_by(& &1.price)
  end

  def cheaper_than(products, price_limit) do
    products
    |> Enum.filter(& &1.active)
    |> Enum.filter(&(&1.price < price_limit))
    |> Enum.sort_by(& &1.price)
  end
end
```
