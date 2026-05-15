smell_id:untested_polymorphic_behaviors

# Example 01

```elixir
defmodule Slugify do
  @doc """
  Turns incoming data into a URL slug.
  The function accepts any value and relies on String.Chars.
  """
  def slugify(data) do
    data
    |> to_string()
    |> String.downcase()
    |> String.replace(" ", "-")
  end
end

# ...Use examples...

iex> Slugify.slugify("Hello World")
"hello-world"

iex> Slugify.slugify(123)
"123"

iex> Slugify.slugify(URI.parse("https://example.com/Some Path"))
"https://example.com/some-path"

iex> Slugify.slugify(%{title: "Hello World"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{title: "Hello World"} of type Map
```
