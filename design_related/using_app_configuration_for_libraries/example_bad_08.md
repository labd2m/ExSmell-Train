id:uacl_08
smell_id:using_app_configuration_for_libraries

# Example 08 - Currency formatter coupled to a single configured locale

```elixir
# config/config.exs
import Config

config :money_format,
  locale: :en,
  symbol: "$"
```

```elixir
defmodule MoneyFormat do
  def format(amount) when is_number(amount) do
    locale = Application.fetch_env!(:money_format, :locale)
    symbol = Application.fetch_env!(:money_format, :symbol)

    case locale do
      :en -> symbol <> :erlang.float_to_binary(amount * 1.0, decimals: 2)
      :pt_br -> :erlang.float_to_binary(amount * 1.0, decimals: 2) <> " " <> symbol
    end
  end
end
```

```elixir
defmodule Invoices do
  def total(amount), do: MoneyFormat.format(amount)
end

defmodule BrazilianStorefront do
  def total(amount), do: MoneyFormat.format(amount)
end
```

The library cannot be reused naturally by applications or modules that need to
format values differently in different parts of the same codebase.
