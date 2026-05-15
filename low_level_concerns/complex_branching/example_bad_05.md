smell_id:complex_branching

# Example 05 — CSV import function with multiple parsing and persistence branches

```elixir
defmodule MyApp.Importers.Users do
  alias MyApp.Repo
  alias MyApp.Accounts.User

  def import_row(row) do
    case parse_csv_row(row) do
      {:ok, %{"email" => email, "age" => age, "role" => role}} ->
        case Integer.parse(age) do
          {parsed_age, ""} ->
            attrs = %{email: email, age: parsed_age, role: role}

            case Repo.insert(User.changeset(%User{}, attrs)) do
              {:ok, user} ->
                {:ok, user}

              {:error, %Ecto.Changeset{} = changeset} ->
                {:error, {:invalid_user, changeset}}

              other ->
                {:error, {:unexpected_insert_result, other}}
            end

          {_, rest} ->
            {:error, {:invalid_age_suffix, rest}}

          :error ->
            {:error, :invalid_age}
        end

      {:ok, %{"email" => _, "age" => _}} ->
        {:error, :missing_role}

      {:ok, _other_shape} ->
        {:error, :invalid_columns}

      {:error, :enoent} ->
        {:error, :file_not_found}

      {:error, :invalid_encoding} ->
        {:error, :invalid_encoding}

      {:error, reason} ->
        {:error, {:parse_error, reason}}
    end
  end

  defp parse_csv_row(row) do
    # Pretend this is an external CSV parser wrapper.
    {:ok, row}
  end
end
```

This is a bad example because `import_row/1` chains several large decision blocks and becomes responsible for parsing, type conversion, and database error handling.
