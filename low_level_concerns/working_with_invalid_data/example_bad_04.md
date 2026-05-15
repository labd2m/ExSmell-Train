smell_id:working_with_invalid_data
# Example 04 — Passing arbitrary IDs into `Ecto.UUID.dump!/1`

This example accepts any `user_id` and delegates validation to `Ecto.UUID.dump!/1`.

## Bad code

```elixir
defmodule MyApp.Accounts do
  def normalize_user_id(user_id) do
    # expected:
    #   user_id -> UUID string
    #
    # bad smell:
    #   no validation before a bang function from Ecto
    Ecto.UUID.dump!(user_id)
  end
end
```

## Example usage

```elixir
iex> MyApp.Accounts.normalize_user_id("0b7a9f9e-53a1-45d6-a8d1-478b2ef6d829")
<<11, 122, 159, 158, 83, 161, 69, 214, 168, 209, 71, 139, 46, 246, 216, 41>>

iex> MyApp.Accounts.normalize_user_id(123)
** (Ecto.CastError) cannot cast 123 to Ecto.UUID
```

## Why this is a smell

The application boundary should reject invalid IDs explicitly.
Instead, a low-level Ecto error is raised later, which makes the failure mode less clear for callers.
