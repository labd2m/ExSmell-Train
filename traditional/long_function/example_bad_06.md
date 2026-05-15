smell_id:long_function

```elixir
defmodule AuditExporter do
  def export(events, file_path) do
    header = "id,actor,action,target,inserted_at\n"

    body =
      events
      |> Enum.sort_by(& &1.inserted_at)
      |> Enum.map(fn event ->
        actor = String.replace(event.actor, ",", " ")
        action = String.replace(event.action, ",", " ")
        target = String.replace(event.target, ",", " ")
        timestamp = NaiveDateTime.to_iso8601(event.inserted_at)
        "#{event.id},#{actor},#{action},#{target},#{timestamp}"
      end)
      |> Enum.join("\n")

    csv = header <> body <> "\n"
    checksum = :crypto.hash(:sha256, csv) |> Base.encode16(case: :lower)
    wrapped = "# checksum=#{checksum}\n" <> csv

    case File.write(file_path, wrapped) do
      :ok ->
        IO.puts("Wrote audit export to #{file_path}")
        %{path: file_path, checksum: checksum, count: length(events)}

      {:error, reason} ->
        IO.puts("Export failed: #{inspect(reason)}")
        {:error, reason}
    end
  end
end
```
