smell_id:use_instead_of_import

# Example 08 - Status helpers propagated into a notifier

The library should expose explicit helpers or let the client import them directly.
Using `use` hides the dependency and spreads imported names into the caller.

```elixir
defmodule StatusHelpers do
  def status(:ok), do: "ok"
  def status(:error), do: "error"
  def label(text), do: "[status] " <> text
end

defmodule NotificationKit do
  defmacro __using__(_opts) do
    quote do
      import StatusHelpers

      def kit_loaded, do: true
    end
  end

  def kit_loaded, do: true
end

defmodule IncidentNotifier do
  use NotificationKit

  def status(_value) do
    "custom-status"
  end

  def message(result) do
    label(status(result))
  end
end
```

The hidden import of `StatusHelpers` decreases readability because `IncidentNotifier`
does not clearly declare where `label/1` and `status/1` come from. It also creates
a local conflict with `status/1`.
