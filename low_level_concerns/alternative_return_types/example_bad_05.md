smell_id:alternative_return_types

# Example 05 - User lookup returning a struct or a map

```elixir
defmodule MyApp.Accounts do
  alias MyApp.{Repo, User}

  def get_user(id, opts \ []) do
    user = Repo.get!(User, id)

    case opts[:as] do
      :map ->
        %{id: user.id, email: user.email}

      _ ->
        user
    end
  end
end

# Usage
MyApp.Accounts.get_user(1)
# => %MyApp.User{id: 1, email: "lucas@example.com"}

MyApp.Accounts.get_user(1, as: :map)
# => %{id: 1, email: "lucas@example.com"}
```

The optional `:as` parameter changes the return from a domain struct to a plain map.
