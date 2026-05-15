smell_id:untested_polymorphic_behaviors

# Example 09

```elixir
defmodule Filename do
  @doc """
  Appends .txt to any value by converting it to a string.
  Dates, numbers, and URIs are silently accepted as filenames.
  """
  def as_text_file(name_like) do
    to_string(name_like) <> ".txt"
  end
end

# ...Use examples...

iex> Filename.as_text_file("report")
"report.txt"

iex> Filename.as_text_file(404)
"404.txt"

iex> Filename.as_text_file(~D[2026-04-22])
"2026-04-22.txt"

iex> Filename.as_text_file(%{name: "report"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{name: "report"} of type Map
```
