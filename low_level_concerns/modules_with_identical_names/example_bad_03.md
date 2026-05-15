smell_id:modules_with_identical_names

# Example 03

Two teams add modules with the same name in different apps of an umbrella project.

## apps/accounts/lib/shared/config.ex
```elixir
defmodule Shared.Config do
  def load do
    %{source: :accounts, timeout: 5_000}
  end
end
```

## apps/payments/lib/shared/config.ex
```elixir
defmodule Shared.Config do
  def load do
    %{source: :payments, retries: 3}
  end
end
```

## Usage
```elixir
defmodule Accounts.Boot do
  def boot do
    Shared.Config.load()
  end
end

defmodule Payments.Boot do
  def boot do
    Shared.Config.load()
  end
end
```

## Why this smells
Both applications refer to `Shared.Config`, but only one module with that name can stay loaded. One app silently shadows the other module definition.
