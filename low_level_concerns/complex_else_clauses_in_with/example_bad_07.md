smell_id:complex_else_clauses_in_with

# Example 07

```elixir
defmodule MyApp.ReportMailer do
  def deliver_daily_report(email, date) do
    with {:ok, report} <- MyApp.Reports.generate(date),
         {:ok, csv} <- MyApp.Reports.to_csv(report),
         {:ok, message} <- MyApp.Mailer.build_message(email, csv),
         {:ok, _response} <- MyApp.Mailer.deliver(message) do
      :ok
    else
      {:error, :no_data} -> {:error, :empty_report}
      {:error, :invalid_date} -> {:error, :bad_date}
      {:error, :csv_encoding_failed} -> {:error, :serialization_failed}
      {:error, :invalid_recipient} -> {:error, :bad_email}
      {:error, :smtp_unavailable} -> {:error, :delivery_unavailable}
      {:error, :rate_limited} -> {:error, :delivery_deferred}
    end
  end
end
```

The function is responsible for report generation, transformation, message construction,
and delivery, with every failure shape flattened into one `else` branch set.
