smell_id:use_instead_of_import

# Example 02 - View helpers injected through `use`

The client only wants a formatting helper, but `use` pulls extra code and an import
into the module. A plain `import HtmlTextHelpers` would be clearer.

```elixir
defmodule HtmlTextHelpers do
  def titleize(text), do: String.capitalize(text)
  def wrap_tag(tag, text), do: "<#{tag}>#{text}</#{tag}>"
end

defmodule UiLibrary do
  defmacro __using__(_opts) do
    quote do
      import HtmlTextHelpers

      def library_name do
        "ui library"
      end
    end
  end

  def library_name, do: "ui library"
end

defmodule AdminPage do
  use UiLibrary

  def wrap_tag(_tag, _text) do
    {:safe, :custom_markup}
  end

  def page_title(name) do
    wrap_tag("h1", titleize(name))
  end
end
```

Because `UiLibrary` uses `__using__/1` to propagate `import HtmlTextHelpers`,
the client module becomes harder to read and can conflict with local functions
such as `wrap_tag/2`.
