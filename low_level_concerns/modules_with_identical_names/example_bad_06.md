smell_id:modules_with_identical_names

# Example 06

Two formatter files accidentally define the exact same helper module name.

## lib/export/csv/formatter.ex
```elixir
defmodule Export.Formatter do
  def render(row) do
    Enum.join(row, ",")
  end
end
```

## lib/export/xml/formatter.ex
```elixir
defmodule Export.Formatter do
  def render(row) do
    "<row>" <> Enum.map_join(row, "", &"<col>#{&1}</col>") <> "</row>"
  end

  def type do
    :xml
  end
end
```

## Callers
```elixir
defmodule Export.CSV do
  def export(rows), do: Enum.map(rows, &Export.Formatter.render/1)
end

defmodule Export.XML do
  def export(rows), do: Enum.map(rows, &Export.Formatter.render/1)
end
```

## Why this smells
Both export paths rely on a module whose meaning changes after redefinition. The collision makes one format implementation overwrite the other.
