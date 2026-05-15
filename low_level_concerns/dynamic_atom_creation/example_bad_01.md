smell_id:dynamic_atom_creation

# Example 1

```elixir
defmodule MyApp.ResourceIdentifier do
  def normalize_type(type_from_params) do
    String.to_atom(type_from_params)
  end
end

defmodule MyApp.Controller do
  def create(params) do
    type = MyApp.ResourceIdentifier.normalize_type(params["type"])

    case type do
      :book -> {:ok, "creating book"}
      :movie -> {:ok, "creating movie"}
      _ -> {:error, :unknown_type}
    end
  end
end
```

`type_from_params` comes from request data. Every new string can create a new atom.
