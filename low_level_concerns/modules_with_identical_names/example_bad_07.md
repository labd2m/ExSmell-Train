smell_id:modules_with_identical_names

# Example 07

A library dependency and application code define the same module name.

## dependency code
```elixir
defmodule Audit.Logger do
  def log(event) do
    IO.puts("dep: " <> inspect(event))
  end

  def origin do
    :dependency
  end
end
```

## application code
```elixir
defmodule Audit.Logger do
  def log(event) do
    File.write!("audit.log", inspect(event) <> "\n", [:append])
  end

  def origin do
    :application
  end
end
```

## Caller
```elixir
defmodule Audit.Service do
  def record(event) do
    Audit.Logger.log(event)
  end
end
```

## Why this smells
Because both modules are named `Audit.Logger`, BEAM treats them as the same module identity. One implementation replaces the other.
