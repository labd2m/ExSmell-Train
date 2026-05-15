smell_id:use_instead_of_import

# Example 06 - Logging helpers brought in through `use`

The client module only wants helper functions, but a library injects an import using
`use`. This broadens the dependency surface and makes the module harder to reason about.

```elixir
defmodule LogHelpers do
  def log(level, message), do: "[#{level}] " <> message
  def tagged(tag, message), do: "[" <> tag <> "] " <> message
end

defmodule LoggingLibrary do
  defmacro __using__(_opts) do
    quote do
      import LogHelpers

      def library_tag, do: "logging-lib"
    end
  end

  def library_tag, do: "logging-lib"
end

defmodule BillingCommand do
  use LoggingLibrary

  def log(_level, _message) do
    :sent_to_external_sink
  end

  def run do
    log(:info, tagged("billing", "running"))
  end
end
```

A plain `import LogHelpers` would be enough here. Using `use LoggingLibrary` hides
the source of imported functions and collides with the local `log/2`.
