smell_id:working_with_invalid_data
# Example 06 — Hashing invalid data with `:crypto.hash/2`

This example forwards arbitrary data to the Erlang crypto module without checking the input type.

## Bad code

```elixir
defmodule MyApp.Avatar do
  def gravatar_hash(email) do
    # expected:
    #   email -> binary
    #
    # bad smell:
    #   invalid values go straight into :crypto
    :crypto.hash(:md5, email)
    |> Base.encode16(case: :lower)
  end
end
```

## Example usage

```elixir
iex> MyApp.Avatar.gravatar_hash("user@example.com")
"b58996c504c5638798eb6b511e6f49af"

iex> MyApp.Avatar.gravatar_hash(%{email: "user@example.com"})
** (ArgumentError) argument error
    :crypto.hash(:md5, %{email: "user@example.com"})
```

## Why this is a smell

`gravatar_hash/1` should reject non-binary inputs immediately.
Instead, the crash happens inside the crypto layer, which hides the fact that the boundary accepted bad data.
