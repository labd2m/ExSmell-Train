smell_id:comments

# Example 02

```elixir
defmodule Accounts.PasswordPolicy do
  # Validates whether a password is acceptable.
  # Minimum size is 12 characters.
  # Must contain at least one upper case letter and one digit.
  def valid?(password) do
    String.length(password) >= 12 and
      String.match?(password, ~r/[A-Z]/) and
      String.match?(password, ~r/[0-9]/)
  end
end
```
