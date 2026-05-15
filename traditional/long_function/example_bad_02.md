smell_id:long_function

```elixir
defmodule CsvImporter do
  def import_users(path, repo, notifier) do
    IO.puts("Starting import from #{path}")

    {:ok, body} = File.read(path)
    rows = String.split(body, "\n", trim: true)

    Enum.each(rows, fn row ->
      columns = String.split(row, ",")
      first_name = Enum.at(columns, 0)
      last_name = Enum.at(columns, 1)
      email = Enum.at(columns, 2)
      age = String.to_integer(Enum.at(columns, 3))

      normalized_email = email |> String.trim() |> String.downcase()
      status = if age >= 18, do: :adult, else: :minor

      attrs = %{
        first_name: String.trim(first_name),
        last_name: String.trim(last_name),
        email: normalized_email,
        age: age,
        status: status
      }

      case repo.insert(attrs) do
        {:ok, user} ->
          notifier.send_welcome_email(user.email)
          IO.puts("Imported #{user.email}")

        {:error, reason} ->
          IO.puts("Failed to import #{normalized_email}: #{inspect(reason)}")
      end
    end)

    IO.puts("Import completed")
    :ok
  end
end
```
