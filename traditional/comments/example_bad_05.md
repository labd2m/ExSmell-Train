smell_id:comments

# Example 05

```elixir
defmodule Store.CatalogSearch do
  # Searches products by a user-provided term.
  # Matching is case-insensitive.
  # Returns only products whose names contain the term.
  def by_name(products, term) do
    normalized_term = String.downcase(term)

    Enum.filter(products, fn product ->
      product.name
      |> String.downcase()
      |> String.contains?(normalized_term)
    end)
  end
end
```
