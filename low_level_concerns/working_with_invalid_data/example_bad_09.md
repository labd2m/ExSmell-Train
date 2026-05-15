smell_id:working_with_invalid_data
# Example 09 — Building query strings from invalid params

This example delegates all validation to `URI.encode_query/1`.

## Bad code

```elixir
defmodule MyApp.Payments do
  def callback_url(base_url, params) do
    # expected:
    #   base_url -> binary
    #   params   -> map or keyword list with stringifiable values
    #
    # bad smell:
    #   arbitrary values flow into URI.encode_query/1
    base_url <> "?" <> URI.encode_query(params)
  end
end
```

## Example usage

```elixir
iex> MyApp.Payments.callback_url("https://pay.example.com/callback", %{status: "ok", id: 10})
"https://pay.example.com/callback?id=10&status=ok"

iex> MyApp.Payments.callback_url("https://pay.example.com/callback", "status=ok")
** (FunctionClauseError) no function clause matching in URI.encode_query/2
```

## Why this is a smell

The public function accepts any second argument, even though the standard library expects a specific shape.
The crash points into `URI`, not to the missing boundary validation in `callback_url/2`.
