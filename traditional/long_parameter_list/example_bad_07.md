smell_id:long_parameter_list

```elixir
defmodule CalendarBooking do
  def schedule(title, date, start_time, end_time, timezone, host_email, guest_email, meeting_type, send_reminder) do
    %{
      title: title,
      starts_at: {date, start_time, timezone},
      ends_at: {date, end_time, timezone},
      host: host_email,
      guest: guest_email,
      meeting_type: meeting_type,
      send_reminder: send_reminder
    }
  end
end

CalendarBooking.schedule(
  "Architecture Review",
  ~D[2026-05-06],
  ~T[14:00:00],
  ~T[15:00:00],
  "America/Sao_Paulo",
  "lead@example.com",
  "dev@example.com",
  :remote,
  true
)
```
