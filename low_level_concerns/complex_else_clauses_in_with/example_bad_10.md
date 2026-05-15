smell_id:complex_else_clauses_in_with

# Example 10

```elixir
defmodule MyApp.SyncJob do
  use Oban.Worker

  @impl Oban.Worker
  def perform(%Oban.Job{args: %{"account_id" => account_id}}) do
    with {:ok, account} <- MyApp.Accounts.fetch(account_id),
         {:ok, payload} <- MyApp.ExternalAPI.fetch_profile(account.external_id),
         {:ok, attrs} <- MyApp.ExternalAPI.normalize_profile(payload),
         {:ok, _user} <- MyApp.Accounts.update_from_remote(account, attrs) do
      :ok
    else
      {:error, :not_found} -> {:discard, :account_not_found}
      {:error, :unauthorized} -> {:discard, :external_auth_failed}
      {:error, :rate_limited} -> {:snooze, 300}
      {:error, :timeout} -> {:snooze, 60}
      {:error, :invalid_payload} -> {:discard, :bad_remote_data}
      {:error, %Ecto.Changeset{}} -> {:discard, :local_validation_failed}
    end
  end
end
```

The job function combines local lookup, remote API access, normalization, persistence,
and job retry decisions in one `with` with a broad `else` block.
