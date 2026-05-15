smell_id:compile_time_global_configuration

# Example 02 — Mail subject prefix captured in a module attribute

The library depends on global configuration while defining constants in the module body.

```elixir
# config/config.exs
import Config

config :mailer_lib,
  subject_prefix: "[ACME]"

# lib/mailer_lib/subject_builder.ex
defmodule MailerLib.SubjectBuilder do
  @prefix Application.fetch_env!(:mailer_lib, :subject_prefix)

  def build(subject) when is_binary(subject) do
    @prefix <> " " <> subject
  end
end
```

## Use examples

```elixir
iex> MailerLib.SubjectBuilder.build("Invoice available")
"[ACME] Invoice available"
```

## Why this is smelly

A library function is indirectly controlled by compile-time application configuration
because `@prefix` is set in the module body. If the configuration changes later, the module
must be recompiled to observe the new value.
