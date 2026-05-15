smell_id:compile_time_global_configuration

# Example 10 — Template helper reads app env while compiling

This example shows both the config and a library module that compiles application values into
module attributes.

```elixir
# config/config.exs
import Config

config :template_bits,
  greeting_prefix: "Hello",
  greeting_suffix: "!"

# lib/template_bits/greeting.ex
defmodule TemplateBits.Greeting do
  @prefix Application.fetch_env!(:template_bits, :greeting_prefix)
  @suffix Application.fetch_env!(:template_bits, :greeting_suffix)

  def render(name) when is_binary(name) do
    @prefix <> ", " <> name <> @suffix
  end
end
```

## Use examples

```elixir
iex> TemplateBits.Greeting.render("Lucas")
"Hello, Lucas!"
```

## Possible failure during compilation

```text
** (ArgumentError) could not fetch application environment :greeting_prefix
for application :template_bits because the application was not loaded nor configured
```

## Why this is smelly

The library is using the Application Environment at compile-time, not just at runtime.
That can produce warnings or hard compilation failures and makes the code less robust.
