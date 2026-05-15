smell_id:compile_time_global_configuration

# Example 08 — Feature flags embedded as module attributes

This library decides behavior based on feature flags loaded in the module body.

```elixir
# config/config.exs
import Config

config :feature_lib,
  allow_legacy_mode: true,
  emit_debug_headers: false

# lib/feature_lib/request_options.ex
defmodule FeatureLib.RequestOptions do
  @legacy_mode Application.fetch_env!(:feature_lib, :allow_legacy_mode)
  @debug_headers Application.fetch_env!(:feature_lib, :emit_debug_headers)

  def build do
    []
    |> maybe_put_legacy()
    |> maybe_put_debug_headers()
  end

  defp maybe_put_legacy(opts) do
    if @legacy_mode, do: Keyword.put(opts, :legacy_mode, true), else: opts
  end

  defp maybe_put_debug_headers(opts) do
    if @debug_headers, do: Keyword.put(opts, :debug_headers, true), else: opts
  end
end
```

## Why this is smelly

The library turns environment configuration into compile-time constants. That means changing
the flags does not simply change runtime behavior; it may require recompilation and can
cause warnings or errors during compilation.
