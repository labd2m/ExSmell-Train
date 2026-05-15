id:uacl_04
smell_id:using_app_configuration_for_libraries

# Example 04 - HTTP retry helper configured only through application env

```elixir
# config/config.exs
import Config

config :retrying_client,
  max_retries: 5,
  retryable_statuses: [429, 500, 502, 503]
```

```elixir
defmodule RetryingClient do
  def get(url) when is_binary(url) do
    max_retries = Application.fetch_env!(:retrying_client, :max_retries)
    statuses = Application.fetch_env!(:retrying_client, :retryable_statuses)

    do_get(url, max_retries, statuses)
  end

  defp do_get(_url, 0, _statuses), do: {:error, :too_many_attempts}

  defp do_get(url, attempts_left, statuses) do
    case HTTPoison.get(url) do
      {:ok, %{status_code: code} = response} when code in statuses ->
        do_get(url, attempts_left - 1, statuses)

      other ->
        other
    end
  end
end
```

```elixir
defmodule WebhookSender do
  def ping(url), do: RetryingClient.get(url)
end

defmodule AdminConsole do
  def check(url), do: RetryingClient.get(url)
end
```

The same library function is reused in two different contexts, but both are
forced to share identical retry policy because it is hidden in app config.
