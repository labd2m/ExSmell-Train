smell_id:complex_branching

# Example 09 — Authentication function branching across many failure cases

```elixir
defmodule MyApp.Auth do
  alias MyApp.{Repo, User}
  alias Comeonin.Bcrypt

  def authenticate(email, password, otp_code) do
    case Repo.get_by(User, email: email) do
      nil ->
        {:error, :not_found}

      %User{disabled_at: disabled_at} when not is_nil(disabled_at) ->
        {:error, :disabled}

      %User{confirmed_at: nil} ->
        {:error, :unconfirmed}

      %User{locked_until: locked_until} when not is_nil(locked_until) ->
        {:error, :locked}

      %User{} = user ->
        case Bcrypt.check_pass(user, password) do
          {:ok, verified_user} ->
            case verify_otp(verified_user, otp_code) do
              :ok ->
                {:ok, verified_user}

              {:error, :missing_otp} ->
                {:error, :missing_otp}

              {:error, :invalid_otp} ->
                {:error, :invalid_otp}

              {:error, :expired_otp} ->
                {:error, :expired_otp}

              {:error, reason} ->
                {:error, {:otp_error, reason}}
            end

          {:error, :invalid} ->
            {:error, :invalid_password}

          {:error, %Ecto.Changeset{} = changeset} ->
            {:error, {:password_check_changeset, changeset}}

          {:error, reason} ->
            {:error, {:hashing_error, reason}}
        end
    end
  end

  defp verify_otp(_user, nil), do: {:error, :missing_otp}
  defp verify_otp(_user, "000000"), do: {:error, :expired_otp}
  defp verify_otp(_user, "123456"), do: :ok
  defp verify_otp(_user, _), do: {:error, :invalid_otp}
end
```

This is a bad example because `authenticate/3` tries to coordinate account state checks, password verification, and OTP validation in a single branching-heavy function.
