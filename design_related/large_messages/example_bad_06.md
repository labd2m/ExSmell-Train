smell_id:large_messages
example_id:example_bad_06

# Example 06 - Report builder sends a huge report structure to the mailer

```elixir
defmodule MailerWorker do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:deliver_report, report} ->
        report.title
        length(report.rows)
        loop()
    end
  end
end

defmodule ReportService do
  def email_monthly_report(email, mailer_pid) do
    report = %{
      to: email,
      title: "Monthly activity report",
      rows:
        Enum.map(1..1_000_000, fn n ->
          %{date: "2026-04-01", action: "view", count: n}
        end)
    }

    # Bad smell: the entire report payload is copied to the mailer process
    send(mailer_pid, {:deliver_report, report})
  end
end

# Use example:
mailer = MailerWorker.start()
ReportService.email_monthly_report("ops@example.com", mailer)
```

The service materializes a giant report and sends the full payload between processes in one shot.
