smell_id:use_instead_of_import

# Example 05 - Mail subject helpers smuggled in via `use`

This example shows a library that could expose normal helper functions, but instead
uses `__using__/1` to import another module into clients.

```elixir
defmodule MailTextHelpers do
  def subject(prefix, text), do: "[" <> prefix <> "] " <> text
  def signature(name), do: "Regards, " <> name
end

defmodule MailComposer do
  defmacro __using__(_opts) do
    quote do
      import MailTextHelpers

      def composer_loaded?, do: true
    end
  end

  def composer_loaded?, do: true
end

defmodule WelcomeMailer do
  use MailComposer

  def subject(_prefix, _text) do
    "custom subject"
  end

  def build(name) do
    %{
      subject: subject("welcome", "Hello " <> name),
      body: signature("Support")
    }
  end
end
```

The client does not visibly declare an `import MailTextHelpers`, so a reader must
inspect `MailComposer` to understand why `subject/2` and `signature/1` are available.
That hidden import also conflicts with the local `subject/2`.
