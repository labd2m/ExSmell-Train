smell_id:use_instead_of_import

# Example 01 - Hidden import of validation helpers

This example shows a library using `use` only to make helper functions available.
A simple `import UserNameRules` in the client module would be enough, but the library
injects code and propagates an import instead. This makes the dependency less obvious
and can create local conflicts.

```elixir
defmodule UserNameRules do
  def normalize(name), do: String.trim(name)
  def present?(name), do: name != ""
end

defmodule ValidationLibrary do
  defmacro __using__(_opts) do
    quote do
      import UserNameRules

      def from_library do
        "validation library loaded"
      end
    end
  end

  def from_library do
    "validation library loaded"
  end
end

defmodule Accounts.Registration do
  use ValidationLibrary

  def normalize(_name) do
    :custom_normalizer
  end

  def valid_name?(name) do
    present?(name) and normalize(name) != ""
  end
end
```

The problem is that `Accounts.Registration` only needs direct access to helper
functions, but `use ValidationLibrary` injects an `import UserNameRules`.
That hidden dependency conflicts with the local `normalize/1`.
