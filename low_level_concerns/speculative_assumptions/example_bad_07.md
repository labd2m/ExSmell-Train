smell_id:speculative_assumptions

# Example 07 - Controller delegates malformed data to a speculative parser

```elixir
defmodule MyApp.Filters do
  @doc """
  Extracts the min price from a query fragment like `min=10-max=20`.

  The code assumes the left side always contains a single `=`.
  """
  def min_price(filter) do
    filter
    |> String.split("-")
    |> Enum.at(0)
    |> String.split("=")
    |> Enum.at(1)
  end
end

defmodule MyApp.ProductController do
  def index(params) do
    min = MyApp.Filters.min_price(params["price"])
    {:ok, %{min_price: min}}
  end
end

# Planned input
MyApp.ProductController.index(%{"price" => "min=10-max=20"})
# => {:ok, %{min_price: "10"}}

# Unplanned input still returns a value, but it is incorrect
MyApp.ProductController.index(%{"price" => "min=currency=10-max=20"})
# => {:ok, %{min_price: "currency"}}
```
