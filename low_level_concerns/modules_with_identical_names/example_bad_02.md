smell_id:modules_with_identical_names

# Example 02

A hand-written module and a generated module accidentally share the same name, so whichever file is compiled last wins.

## lib/inventory/serializer.ex
```elixir
defmodule Inventory.Serializer do
  def encode(item) do
    "#{item.id}:#{item.name}"
  end
end
```

## _build/dev/generated/serializer.ex
```elixir
defmodule Inventory.Serializer do
  def encode(item) do
    Jason.encode!(item)
  end

  def format do
    :json
  end
end
```

## Caller
```elixir
defmodule Inventory.Sync do
  def push(item) do
    Inventory.Serializer.encode(item)
  end
end
```

## Why this smells
`Inventory.Sync.push/1` depends on one specific implementation, but BEAM can only keep one `Inventory.Serializer` loaded. The active behavior changes depending on which file redefines the module last.
