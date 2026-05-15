smell_id:untested_polymorphic_behaviors

# Example 05

```elixir
defmodule MessageTag do
  @doc """
  Prefixes a label with # for logging and chat usage.
  Any String.Chars value goes through, even when it makes no business sense.
  """
  def tag(value) do
    "#" <> to_string(value)
  end
end

# ...Use examples...

iex> MessageTag.tag("deploy")
"#deploy"

iex> MessageTag.tag(2026)
"#2026"

iex> MessageTag.tag(~T[12:30:00])
"#12:30:00"

iex> MessageTag.tag(%{team: "platform"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{team: "platform"} of type Map
```
