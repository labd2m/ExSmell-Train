smell_id:unrelated_multi_clause_function

# Example 03

The function `update/1` handles **product inventory changes**, **animal adoption state**, and **marketing campaign status**, which are unrelated domains.

```elixir
defmodule MyApp.UpdateCenter do
  defmodule Product do
    defstruct [:sku, :count, :material]
  end

  defmodule Animal do
    defstruct [:name, :count, :species]
  end

  defmodule Campaign do
    defstruct [:name, :clicks, :status]
  end

  @doc """
  Update records.
  """
  def update(%Product{count: count, material: material} = product)
      when is_integer(count) and material in ["metal", "glass"] do
    %{product | count: count + 1}
  end

  # unrelated: animal shelter logic
  def update(%Animal{count: 1, species: species} = animal)
      when species in ["cat", "dog"] do
    %{animal | count: 0}
  end

  # unrelated: marketing logic
  def update(%Campaign{clicks: clicks, status: :draft} = campaign) when clicks > 1000 do
    %{campaign | status: :live}
  end
end
```
