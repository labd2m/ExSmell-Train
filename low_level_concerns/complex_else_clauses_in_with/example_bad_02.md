smell_id:complex_else_clauses_in_with

# Example 02

```elixir
defmodule MyApp.TokenAuth do
  def authenticate(conn) do
    with [header] <- Plug.Conn.get_req_header(conn, "authorization"),
         "Bearer " <> token <- header,
         {:ok, claims} <- MyApp.Token.verify(token),
         {:ok, user} <- MyApp.Accounts.fetch_active_user(claims["sub"]) do
      {:ok, user}
    else
      [] -> {:error, :missing_header}
      "Basic " <> _ -> {:error, :unsupported_scheme}
      header when is_binary(header) -> {:error, :malformed_header}
      {:error, :expired} -> {:error, :token_expired}
      {:error, :invalid_signature} -> {:error, :invalid_token}
      {:error, :revoked} -> {:error, :revoked_token}
      {:error, :not_found} -> {:error, :user_not_found}
      {:error, :inactive} -> {:error, :inactive_user}
    end
  end
end
```

This function handles HTTP header parsing, token verification, and user lookup in a single `with`.
The `else` block mixes pattern failures with several unrelated domain errors.
