smell_id:working_with_invalid_data
# Example 03 — Controller passes invalid params to `Timex`

In this case the smell is not limited to one local function.  
The controller accepts request params and forwards them into a service that calls `Timex.parse!/3` without validating the type first.

## Bad code

```elixir
defmodule MyAppWeb.EventController do
  use MyAppWeb, :controller
  alias MyApp.CalendarService

  def create(conn, params) do
    # params["occurred_at"] is expected to be a datetime string,
    # but nothing validates that assumption.
    event = CalendarService.build_event(params["occurred_at"])
    json(conn, %{event: event})
  end
end

defmodule MyApp.CalendarService do
  def build_event(occurred_at) do
    # expected:
    #   occurred_at -> binary like "2026-04-22 14:30:00"
    #
    # bad smell:
    #   invalid data flows straight into Timex
    parsed = Timex.parse!(occurred_at, "{YYYY}-{0M}-{0D} {h24}:{m}:{s}")

    %{
      occurred_at: parsed,
      kind: "user_action"
    }
  end
end
```

## Example usage

```elixir
# valid controller input
params = %{"occurred_at" => "2026-04-22 14:30:00"}

# invalid controller input
params = %{"occurred_at" => %{year: 2026, month: 4, day: 22}}

# representative failure
** (FunctionClauseError) no function clause matching in Timex.Parse.DateTime.Parser.parse!/3
```

## Why this is a smell

The real boundary is the controller, but the crash only happens when the service reaches `Timex`.
A developer debugging the issue may start inside the date parsing library instead of the controller input contract.
