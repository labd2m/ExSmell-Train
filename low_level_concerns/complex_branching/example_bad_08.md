smell_id:complex_branching

# Example 08 — Report generation flow with several nested result forms

```elixir
defmodule MyApp.Reports do
  alias MyApp.Repo

  def generate_monthly_report(month) do
    case validate_month(month) do
      :ok ->
        case fetch_report_rows(month) do
          {:ok, rows} when rows == [] ->
            {:error, :empty_report}

          {:ok, rows} ->
            case render_csv(rows) do
              {:ok, csv} ->
                case File.write("/tmp/report-#{month}.csv", csv) do
                  :ok -> {:ok, "/tmp/report-#{month}.csv"}
                  {:error, :enospc} -> {:error, :disk_full}
                  {:error, :eacces} -> {:error, :permission_denied}
                  {:error, reason} -> {:error, {:file_write_error, reason}}
                end

              {:error, :too_many_columns} ->
                {:error, :render_failed}

              {:error, reason} ->
                {:error, {:csv_render_error, reason}}
            end

          {:error, :timeout} ->
            {:error, :db_timeout}

          {:error, :closed} ->
            {:error, :db_connection_closed}

          {:error, reason} ->
            {:error, {:query_error, reason}}
        end

      {:error, :invalid_format} ->
        {:error, :invalid_month}

      {:error, :future_month} ->
        {:error, :future_month}

      {:error, reason} ->
        {:error, {:validation_error, reason}}
    end
  end

  defp validate_month(month) do
    if Regex.match?(~r/^\d{4}-\d{2}$/, month), do: :ok, else: {:error, :invalid_format}
  end

  defp fetch_report_rows(_month) do
    {:ok, [%{id: 1, amount: 10}]}
  end

  defp render_csv(rows) do
    {:ok, inspect(rows)}
  end
end
```

This is a bad example because `generate_monthly_report/1` is dominated by error orchestration rather than business intent, with multiple nested `case` blocks.
