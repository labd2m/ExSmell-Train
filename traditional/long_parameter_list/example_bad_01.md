smell_id:long_parameter_list

```elixir
defmodule Library do
  @moduledoc false

  def loan(user_name, email, password, user_alias, book_name, book_ed, active) do
    if active do
      %{
        borrower: user_name,
        alias: user_alias,
        credentials: {email, password},
        book: "#{book_name} (#{book_ed})",
        status: :loaned
      }
    else
      {:error, :inactive_user}
    end
  end
end

# Use examples
Library.loan(
  "Lucas Vegi",
  "lucas@example.com",
  "secret123",
  "lvegi",
  "Domain Modeling Made Functional",
  2,
  true
)
```
