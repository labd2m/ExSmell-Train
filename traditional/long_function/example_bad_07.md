smell_id:long_function

```elixir
defmodule SignupFlow do
  def register(params, repo, mailer) do
    email = params["email"] |> String.trim() |> String.downcase()
    password = params["password"]
    first_name = params["first_name"] |> String.trim()
    last_name = params["last_name"] |> String.trim()
    marketing = params["marketing"] == "true"

    if first_name == "" or last_name == "" do
      raise "name is required"
    end

    if String.length(password) < 12 do
      raise "password too short"
    end

    hashed_password = :crypto.hash(:sha256, password) |> Base.encode16(case: :lower)
    token = :crypto.strong_rand_bytes(16) |> Base.url_encode64(padding: false)

    attrs = %{
      email: email,
      first_name: first_name,
      last_name: last_name,
      marketing_opt_in: marketing,
      hashed_password: hashed_password,
      confirmation_token: token
    }

    case repo.insert(attrs) do
      {:ok, user} ->
        mailer.send_confirmation(user.email, token)
        IO.puts("User #{user.email} registered")
        {:ok, user}

      {:error, reason} ->
        IO.puts("Registration failed: #{inspect(reason)}")
        {:error, reason}
    end
  end
end
```
