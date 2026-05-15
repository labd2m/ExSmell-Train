smell_id:working_with_invalid_data
# Example 05 — Service forwards invalid HTTP body to `HTTPoison`

Here the smell crosses module boundaries.  
A public notifier API accepts any payload and forwards it to an HTTP client that expects encoded request data.

## Bad code

```elixir
defmodule MyApp.WebhookNotifier do
  alias MyApp.HTTPClient

  def notify(url, payload) do
    # expected:
    #   url     -> binary
    #   payload -> JSON string
    #
    # bad smell:
    #   payload is not validated or encoded here
    HTTPClient.post_json(url, payload)
  end
end

defmodule MyApp.HTTPClient do
  def post_json(url, payload) do
    HTTPoison.post!(
      url,
      payload,
      [{"content-type", "application/json"}]
    )
  end
end
```

## Example usage

```elixir
iex> MyApp.WebhookNotifier.notify("https://api.example.com/hooks", "{\"ok\":true}")
%HTTPoison.Response{...}

iex> MyApp.WebhookNotifier.notify("https://api.example.com/hooks", %{ok: true})
# representative failure, often raised from Hackney/HTTPoison internals
** (ArgumentError) argument error
```

## Why this is a smell

The API boundary accepts any payload, even though the lower-level client expects encoded data.
When it fails, the developer sees a transport-layer exception instead of a clear validation error from `notify/2`.
