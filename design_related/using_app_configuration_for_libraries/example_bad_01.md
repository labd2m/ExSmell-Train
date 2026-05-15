id:uacl_01
smell_id:using_app_configuration_for_libraries

# Example 01 - Global split parts in a string utility library

```elixir
# config/config.exs
import Config

config :text_utils,
  split_parts: 3
```

```elixir
defmodule TextUtils.DashSplitter do
  @moduledoc """
  Library function whose behavior is controlled by the Application Environment.
  Every caller gets the same split size, even when different contexts need
  different behavior.
  """

  def split(value) when is_binary(value) do
    parts = Application.fetch_env!(:text_utils, :split_parts)
    String.split(value, "-", parts: parts)
  end
end
```

```elixir
defmodule BackofficeImporter do
  alias TextUtils.DashSplitter

  def parse_employee_code(code) do
    DashSplitter.split(code)
  end
end

defmodule AuditLogParser do
  alias TextUtils.DashSplitter

  def parse_event_code(code) do
    DashSplitter.split(code)
  end
end
```

```elixir
iex> BackofficeImporter.parse_employee_code("sales-latam-brazil-042")
["sales", "latam", "brazil-042"]

iex> AuditLogParser.parse_event_code("auth-session-user-expired")
["auth", "session", "user-expired"]
```

Both callers are forced to use the same globally configured split behavior,
although they belong to unrelated contexts and may need different numbers of
parts.
