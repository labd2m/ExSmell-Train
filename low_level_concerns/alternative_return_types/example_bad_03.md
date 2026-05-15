smell_id:alternative_return_types

# Example 03 - HTTP client returning decoded JSON or a full response struct

```elixir
defmodule MyApp.Http do
  def get(url, opts \ []) do
    response = Tesla.get!(url)

    case opts[:decode_json] do
      true ->
        Jason.decode!(response.body)

      _ ->
        response
    end
  end
end

# Usage
MyApp.Http.get("https://api.example.com/users")
# => %Tesla.Env{status: 200, body: "{...}"}

MyApp.Http.get("https://api.example.com/users", decode_json: true)
# => [%{"id" => 1, "name" => "Lucas"}]
```

A caller cannot know whether `get/2` returns `%Tesla.Env{}` or decoded JSON without tracking the options passed in.
