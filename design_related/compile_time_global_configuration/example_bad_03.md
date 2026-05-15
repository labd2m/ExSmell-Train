smell_id:compile_time_global_configuration

# Example 03 — Retry policy frozen at compile-time

This client compiles retry behavior into module attributes.

```elixir
# config/config.exs
import Config

config :api_wrapper,
  retry_attempts: 5,
  retry_backoff_ms: 250

# lib/api_wrapper/client.ex
defmodule ApiWrapper.Client do
  @attempts Application.fetch_env!(:api_wrapper, :retry_attempts)
  @backoff_ms Application.fetch_env!(:api_wrapper, :retry_backoff_ms)

  def request(fun) when is_function(fun, 0) do
    do_request(fun, @attempts)
  end

  defp do_request(fun, 1), do: fun.()

  defp do_request(fun, attempts_left) do
    case fun.() do
      {:ok, value} ->
        {:ok, value}

      {:error, _reason} ->
        Process.sleep(@backoff_ms)
        do_request(fun, attempts_left - 1)
    end
  end
end
```

## Why this is smelly

The retry policy is not simply configured globally; it is also embedded into the module
during compilation. This makes the library harder to reuse and can produce compile-time
warnings or boot-order problems.
