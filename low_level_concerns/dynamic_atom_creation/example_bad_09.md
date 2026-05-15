smell_id:dynamic_atom_creation

# Example 9

```elixir
defmodule MyApp.JsonDecoder do
  def decode_and_symbolize(json) do
    {:ok, data} = Jason.decode(json)

    for {key, value} <- data, into: %{} do
      {String.to_atom(key), value}
    end
  end
end

# Example JSON:
# ~s({"email":"a@example.com","plan":"gold","custom_key_999":"abc"})
```

Decoded JSON keys are runtime data. Converting them all to atoms can slowly exhaust the atom table.
