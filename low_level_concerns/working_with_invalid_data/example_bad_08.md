smell_id:working_with_invalid_data
# Example 08 — Phoenix controller forwards invalid content to `Earmark`

The smell spans more than one function: a controller accepts user input and passes it into a renderer that assumes it got a markdown string.

## Bad code

```elixir
defmodule MyAppWeb.DocsController do
  use MyAppWeb, :controller
  alias MyApp.MarkdownRenderer

  def preview(conn, params) do
    html = MarkdownRenderer.render!(params["content"])
    json(conn, %{html: html})
  end
end

defmodule MyApp.MarkdownRenderer do
  def render!(markdown) do
    # expected:
    #   markdown -> binary
    #
    # bad smell:
    #   no validation at the controller or renderer boundary
    Earmark.as_html!(markdown)
  end
end
```

## Example usage

```elixir
# valid
params = %{"content" => "# Hello"}

# invalid
params = %{"content" => %{title: "Hello"}}

# representative failure
** (FunctionClauseError) no function clause matching in Earmark.Parser.parse/2
```

## Why this is a smell

The renderer trusts the controller input blindly.
The resulting exception happens inside the markdown parser, which obscures the real issue: invalid request data was accepted upstream.
