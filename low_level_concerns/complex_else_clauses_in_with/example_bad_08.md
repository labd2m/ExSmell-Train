smell_id:complex_else_clauses_in_with

# Example 08

```elixir
defmodule MyApp.UserController do
  use MyAppWeb, :controller

  def create(conn, params) do
    with {:ok, attrs} <- normalize_params(params),
         {:ok, user} <- MyApp.Accounts.register_user(attrs),
         {:ok, token} <- MyApp.Token.issue(user) do
      json(conn, %{id: user.id, token: token})
    else
      {:error, :missing_email} -> send_resp(conn, 400, "email is required")
      {:error, :missing_password} -> send_resp(conn, 400, "password is required")
      {:error, %Ecto.Changeset{} = changeset} ->
        conn
        |> put_status(422)
        |> json(%{errors: translate_errors(changeset)})

      {:error, :weak_password} -> send_resp(conn, 422, "weak password")
      {:error, :email_taken} -> send_resp(conn, 409, "email already in use")
      {:error, :signing_key_unavailable} -> send_resp(conn, 500, "token service unavailable")
    end
  end

  defp normalize_params(%{"email" => email, "password" => password}) do
    {:ok, %{email: String.trim(email), password: password}}
  end

  defp normalize_params(%{"password" => _}), do: {:error, :missing_email}
  defp normalize_params(%{"email" => _}), do: {:error, :missing_password}
  defp normalize_params(_), do: {:error, :missing_email}

  defp translate_errors(changeset), do: changeset.errors
end
```

The controller action mixes parameter normalization, registration, token issuance,
and HTTP response construction inside one `with` and one complicated `else`.
