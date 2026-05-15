smell_id:comments

# Example 09

```elixir
defmodule Media.DurationFormatter do
  # Converts a duration in seconds to HH:MM:SS format.
  # Hours are zero-padded to two digits.
  # This is intended for the admin dashboard.
  def format(seconds) do
    hours = div(seconds, 3600)
    minutes = div(rem(seconds, 3600), 60)
    remaining_seconds = rem(seconds, 60)

    Enum.join([
      String.pad_leading(Integer.to_string(hours), 2, "0"),
      String.pad_leading(Integer.to_string(minutes), 2, "0"),
      String.pad_leading(Integer.to_string(remaining_seconds), 2, "0")
    ], ":")
  end
end
```
