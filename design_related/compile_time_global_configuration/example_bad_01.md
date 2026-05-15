smell_id:compile_time_global_configuration

# Example 01 — CSV parser configured at compile-time

This library reads an application environment value into a module attribute during compilation.

```elixir
# config/config.exs
import Config

config :csv_tools,
  delimiter: ";"

# lib/csv_tools/parser.ex
defmodule CsvTools.Parser do
  @moduledoc """
  Library code that bakes a runtime configuration into a module attribute.
  """

  @delimiter Application.fetch_env!(:csv_tools, :delimiter)

  def split_line(line) when is_binary(line) do
    String.split(line, @delimiter)
  end
end
```

## Use examples

```elixir
iex> CsvTools.Parser.split_line("name;email;role")
["name", "email", "role"]
```

## Why this is smelly

The library is using `Application.fetch_env!/2` in the module body to populate `@delimiter`.
That means the value is captured at compile-time, which can emit warnings or fail if the
application environment is not loaded when the module is compiled.
