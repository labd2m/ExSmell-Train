smell_id:accessing_non_existent_map_struct_fields

# Example 01 - Shipping zone inferred from missing map keys

```elixir
defmodule Checkout do
  def shipping_zone(address) do
    case {address[:country], address[:state]} do
      {"US", nil} -> :domestic_no_state
      {"US", state} when is_binary(state) -> {:domestic, state}
      {nil, _} -> :unknown_country
      {country, _} -> {:international, country}
    end
  end
end

# Use examples
# iex> Checkout.shipping_zone(%{country: "US", state: "CA"})
# {:domestic, "CA"}
#
# iex> Checkout.shipping_zone(%{country: "US"})
# :domestic_no_state
#
# iex> Checkout.shipping_zone(%{country: "US", state: nil})
# :domestic_no_state
```

The smell is that `address[:state]` returns `nil` both when `:state` is absent and when it is explicitly set to `nil`, so the function cannot distinguish malformed input from a real value.
