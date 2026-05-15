smell_id:compile_time_global_configuration

# Example 04 — Slug generator reads separator from app env in module body

```elixir
# config/config.exs
import Config

config :slug_tools,
  separator: "-",
  max_length: 32

# lib/slug_tools/generator.ex
defmodule SlugTools.Generator do
  @separator Application.fetch_env!(:slug_tools, :separator)
  @max_length Application.fetch_env!(:slug_tools, :max_length)

  def generate(title) when is_binary(title) do
    title
    |> String.downcase()
    |> String.replace(~r/[^a-z0-9]+/u, @separator)
    |> String.trim(@separator)
    |> String.slice(0, @max_length)
  end
end
```

## Use examples

```elixir
iex> SlugTools.Generator.generate("Large Compile-Time Config Example!")
"large-compile-time-config-examp"
```

## Why this is smelly

The library couples behavior to compile-time configuration by assigning app env values to
module attributes. The generated slug behavior becomes frozen until recompilation.
