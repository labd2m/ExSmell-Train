smell_id:shotgun_surgery

# Shotgun Surgery - Example 06

```elixir
defmodule SearchBoosts do
  def weight(:name), do: 5
  def weight(:description), do: 1
  # def weight(:brand), do: 3
end

defmodule SearchFilters do
  def allowed(:name), do: true
  def allowed(:description), do: true
  # def allowed(:brand), do: true
end

defmodule SearchHighlight do
  def snippet_size(:name), do: 20
  def snippet_size(:description), do: 120
  # def snippet_size(:brand), do: 40
end

defmodule SearchQuery do
  def build(field, term) do
    %{
      field: field,
      term: term,
      weight: SearchBoosts.weight(field),
      highlight: SearchHighlight.snippet_size(field)
    }
  end
end
```
