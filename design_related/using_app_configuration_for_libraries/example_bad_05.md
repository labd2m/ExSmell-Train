id:uacl_05
smell_id:using_app_configuration_for_libraries

# Example 05 - JSON formatting library with globally fixed style

```elixir
# config/config.exs
import Config

config :json_view,
  pretty: true,
  escape: :html_safe
```

```elixir
defmodule JsonView do
  def encode(map) when is_map(map) do
    pretty = Application.fetch_env!(:json_view, :pretty)
    escape = Application.fetch_env!(:json_view, :escape)

    Jason.encode!(map, pretty: pretty, escape: escape)
  end
end
```

```elixir
defmodule ApiResponse do
  def render(data), do: JsonView.encode(data)
end

defmodule SignedWebhookBody do
  def render(data), do: JsonView.encode(data)
end
```

```elixir
iex> ApiResponse.render(%{status: "ok"})
"{\n  \"status\": \"ok\"\n}"

iex> SignedWebhookBody.render(%{status: "ok"})
"{\n  \"status\": \"ok\"\n}"
```

One caller may want pretty JSON for humans and another compact JSON for signing,
but the library hardcodes the behavior by reading global configuration.
