id:uacl_03
smell_id:using_app_configuration_for_libraries

# Example 03 - Slug generation library coupled to app-wide settings

```elixir
# config/config.exs
import Config

config :slug_lib,
  separator: "_",
  lowercase: false
```

```elixir
defmodule SlugLib do
  def generate(text) when is_binary(text) do
    separator = Application.fetch_env!(:slug_lib, :separator)
    lowercase = Application.fetch_env!(:slug_lib, :lowercase)

    value =
      if lowercase do
        String.downcase(text)
      else
        text
      end

    value
    |> String.replace(~r/\s+/, separator)
    |> String.replace(~r/[^a-zA-Z0-9_\-]/, "")
  end
end
```

```elixir
defmodule BlogPostUrls do
  def build(title), do: SlugLib.generate(title)
end

defmodule S3Keys do
  def build(filename), do: SlugLib.generate(filename)
end
```

```elixir
iex> BlogPostUrls.build("My First Post")
"My_First_Post"

iex> S3Keys.build("Quarterly Report 2026.pdf")
"Quarterly_Report_2026pdf"
```

The library forces one global formatting style for every call site, even though
URL slugs and storage keys frequently require different formatting rules.
