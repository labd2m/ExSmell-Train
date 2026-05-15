id:uacl_09
smell_id:using_app_configuration_for_libraries

# Example 09 - Markdown renderer with behavior hidden in application config

```elixir
# config/config.exs
import Config

config :markdown_tools,
  sanitize: true,
  hardbreaks: false
```

```elixir
defmodule MarkdownTools do
  def to_html(markdown) when is_binary(markdown) do
    sanitize = Application.fetch_env!(:markdown_tools, :sanitize)
    hardbreaks = Application.fetch_env!(:markdown_tools, :hardbreaks)

    markdown
    |> Earmark.as_html!(breaks: hardbreaks)
    |> maybe_sanitize(sanitize)
  end

  defp maybe_sanitize(html, true), do: HtmlSanitizeEx.basic_html(html)
  defp maybe_sanitize(html, false), do: html
end
```

```elixir
defmodule PublicDocs do
  def render(md), do: MarkdownTools.to_html(md)
end

defmodule InternalPreview do
  def render(md), do: MarkdownTools.to_html(md)
end
```

A library function that could be reused flexibly now has one global rendering
policy shared by every caller.
