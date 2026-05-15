smell_id:untested_polymorphic_behaviors

# Example 08

```elixir
defmodule Audit do
  @doc """
  Turns any value into log text.
  It relies on the Inspect protocol without defining what inputs are meaningful.
  """
  def summarize(data) do
    inspect(data, limit: 5)
  end
end

# ...Use examples...

iex> Audit.summarize(%{event: "login", user_id: 1})
"%{event: \"login\", user_id: 1}"

iex> Audit.summarize(self())
"#PID<0.123.0>"

iex> Audit.summarize(make_ref())
"#Reference<...>"

iex> Audit.summarize(fn -> :ok end)
"#Function<...>"
```
