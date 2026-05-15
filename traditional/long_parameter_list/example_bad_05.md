smell_id:long_parameter_list

```elixir
defmodule Accounts do
  def register_user(first_name, last_name, email, password, password_confirmation, role, timezone, marketing_opt_in) do
    if password == password_confirmation do
      %{
        name: "#{first_name} #{last_name}",
        email: email,
        role: role,
        timezone: timezone,
        marketing_opt_in: marketing_opt_in
      }
    else
      {:error, :password_mismatch}
    end
  end
end

Accounts.register_user(
  "João",
  "Silva",
  "joao@example.com",
  "12345678",
  "12345678",
  :admin,
  "America/Sao_Paulo",
  false
)
```
