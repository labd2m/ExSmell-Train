smell_id:compile_time_global_configuration

# Example 05 — JSON encoder indentation fixed at compile-time

```elixir
# config/config.exs
import Config

config :json_formatter,
  indent: 4

# lib/json_formatter/encoder.ex
defmodule JsonFormatter.Encoder do
  @indent Application.fetch_env!(:json_formatter, :indent)

  def encode(map) when is_map(map) do
    Jason.encode_to_iodata!(map, pretty: true, escape: :json)
    |> IO.iodata_to_binary()
    |> String.replace("\n", "\n" <> String.duplicate(" ", @indent))
  end
end
```

## Why this is smelly

The formatting behavior is set through a compile-time module attribute loaded from the
Application Environment. This makes the library less predictable and may trigger warnings
about reading application config in the module body.
