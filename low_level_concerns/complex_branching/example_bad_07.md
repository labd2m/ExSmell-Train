# Example 7 — Complex branching

This example intentionally demonstrates the **Complex branching** smell.
The function `sync_user_profile/2` concentrates validation, database lookup, remote API handling, and side effects in a single `with` + `case` flow, producing many branches and making the code brittle.

```elixir
defmodule Accounts.ProfileSync do
  alias MyApp.{Repo, AuditLog}
  alias MyApp.Accounts.User

  def sync_user_profile(user_id, attrs) do
    with %User{} = user <- Repo.get(User, user_id),
         :ok <- validate_attrs(attrs) do
      case ExternalProfileAPI.update_profile(user.external_id, attrs) do
        {:ok, %{status: 200, body: %{"name" => name, "email" => email} = body}} ->
          changeset = User.changeset(user, %{name: name, email: email, synced_at: DateTime.utc_now()})

          case Repo.update(changeset) do
            {:ok, updated_user} ->
              AuditLog.log(:profile_synced, updated_user.id, body)
              {:ok, updated_user}

            {:error, changeset} ->
              AuditLog.log(:local_update_failed, user.id, changeset.errors)
              {:error, {:db_update_failed, changeset}}
          end

        {:ok, %{status: 200, body: %{"pending_review" => true}}} ->
          AuditLog.log(:profile_pending_review, user.id, attrs)
          {:error, :pending_review}

        {:ok, %{status: 202, body: %{"ticket" => ticket}}} ->
          AuditLog.log(:profile_sync_queued, user.id, %{ticket: ticket})
          {:queued, ticket}

        {:ok, %{status: 400, body: %{"errors" => errors}}} ->
          AuditLog.log(:remote_validation_failed, user.id, errors)
          {:error, {:remote_validation_failed, errors}}

        {:ok, %{status: 401}} ->
          AuditLog.log(:remote_unauthorized, user.id, %{})
          {:error, :unauthorized}

        {:ok, %{status: 403, body: %{"message" => message}}} ->
          AuditLog.log(:remote_forbidden, user.id, %{message: message})
          {:error, {:forbidden, message}}

        {:ok, %{status: 404}} ->
          AuditLog.log(:remote_user_missing, user.id, %{})
          {:error, :remote_user_not_found}

        {:ok, %{status: 409, body: %{"conflict_field" => field}}} ->
          AuditLog.log(:profile_conflict, user.id, %{field: field})
          {:error, {:conflict, field}}

        {:ok, %{status: 422, body: %{"details" => details}}} ->
          AuditLog.log(:unprocessable_remote_profile, user.id, details)
          {:error, {:unprocessable_entity, details}}

        {:ok, %{status: 500, body: body}} ->
          AuditLog.log(:remote_server_error, user.id, body)
          {:error, :remote_server_error}

        {:error, :timeout} ->
          AuditLog.log(:profile_sync_timeout, user.id, attrs)
          {:error, :timeout}

        {:error, :nxdomain} ->
          AuditLog.log(:profile_sync_dns_error, user.id, attrs)
          {:error, :dns_error}

        {:error, reason} ->
          AuditLog.log(:profile_sync_unknown_failure, user.id, %{reason: reason})
          {:error, {:unexpected_remote_error, reason}}
      end
    else
      nil ->
        {:error, :user_not_found}

      {:error, :invalid_email} ->
        {:error, :invalid_email}

      {:error, :invalid_name} ->
        {:error, :invalid_name}

      {:error, reason} ->
        {:error, reason}
    end
  end

  defp validate_attrs(%{"email" => email, "name" => name})
       when is_binary(email) and is_binary(name) and byte_size(name) > 1 do
    cond do
      not String.contains?(email, "@") -> {:error, :invalid_email}
      String.trim(name) == "" -> {:error, :invalid_name}
      true -> :ok
    end
  end

  defp validate_attrs(_), do: {:error, :invalid_payload}
end

defmodule ExternalProfileAPI do
  def update_profile(_external_id, _attrs) do
    # remote HTTP/API call
    raise "stub"
  end
end
```

Why this is a smell:

- `sync_user_profile/2` is responsible for too many kinds of decision-making.
- The function contains nested branching for API responses, DB writes, validation, and audit logging.
- The control flow is long and fragile, so small changes can introduce regressions.
- Testing all combinations becomes expensive and easy to miss.
