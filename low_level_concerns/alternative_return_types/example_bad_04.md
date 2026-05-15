smell_id:alternative_return_types

# Example 04 - Price parser returning integer cents or a tuple

```elixir
defmodule PriceParser do
  def parse(value, opts \ []) do
    cents =
      value
      |> String.replace("$", "")
      |> String.to_float()
      |> Kernel.*(100)
      |> round()

    case opts[:cents] do
      true ->
        cents

      _ ->
        {:ok, cents, "USD"}
    end
  end
end

# Usage
PriceParser.parse("$10.25")
# => {:ok, 1025, "USD"}

PriceParser.parse("$10.25", cents: true)
# => 1025
```

The option changes the return from a tagged tuple to a plain integer.
