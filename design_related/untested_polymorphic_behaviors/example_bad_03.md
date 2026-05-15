smell_id:untested_polymorphic_behaviors

# Example 03

```elixir
defmodule EmailTemplate do
  @doc """
  Builds a greeting line from any `name_like` value.
  Integers and URIs are accepted only because String.Chars allows them.
  """
  def greeting(name_like) do
    "Hello, #{to_string(name_like)}!"
  end
end

# ...Use examples...

iex> EmailTemplate.greeting("Lucas")
"Hello, Lucas!"

iex> EmailTemplate.greeting(77)
"Hello, 77!"

iex> EmailTemplate.greeting(URI.parse("https://company.example/users/lucas"))
"Hello, https://company.example/users/lucas!"

iex> EmailTemplate.greeting(%{first: "Lucas", last: "Vegi"})
** (Protocol.UndefinedError) protocol String.Chars not implemented for %{first: "Lucas", last: "Vegi"} of type Map
```
