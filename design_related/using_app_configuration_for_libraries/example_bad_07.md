id:uacl_07
smell_id:using_app_configuration_for_libraries

# Example 07 - Password policy helper configured globally inside the library

```elixir
# config/config.exs
import Config

config :password_rules,
  min_length: 14,
  require_symbol: true
```

```elixir
defmodule PasswordRules do
  def valid?(password) when is_binary(password) do
    min_length = Application.fetch_env!(:password_rules, :min_length)
    require_symbol = Application.fetch_env!(:password_rules, :require_symbol)

    long_enough? = String.length(password) >= min_length
    has_symbol? = String.match?(password, ~r/[^a-zA-Z0-9]/)

    long_enough? and (not require_symbol or has_symbol?)
  end
end
```

```elixir
defmodule CustomerSignup do
  def check(password), do: PasswordRules.valid?(password)
end

defmodule AdminBootstrap do
  def check(password), do: PasswordRules.valid?(password)
end
```

```elixir
iex> CustomerSignup.check("simple-pass-123")
true

iex> AdminBootstrap.check("simple-pass-123")
true
```

The library prevents one context from using a relaxed policy and another from
using a strict one, because policy is hidden in application env.
