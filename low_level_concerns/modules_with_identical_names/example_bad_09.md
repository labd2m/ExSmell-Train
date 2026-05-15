smell_id:modules_with_identical_names

# Example 09

Code generation inside a macro creates a module with a fixed global name, and using the macro twice redefines it.

## Macro definition
```elixir
defmodule MyApp.Definer do
  defmacro define_adapter(prefix) do
    quote do
      defmodule GlobalAdapter do
        def prefix, do: unquote(prefix)
      end
    end
  end
end
```

## First usage
```elixir
defmodule FirstFeature do
  require MyApp.Definer
  MyApp.Definer.define_adapter("first")
end
```

## Second usage
```elixir
defmodule SecondFeature do
  require MyApp.Definer
  MyApp.Definer.define_adapter("second")
end
```

## Why this smells
The macro expands to the exact same module name, `GlobalAdapter`, every time. The second expansion redefines the first one.
