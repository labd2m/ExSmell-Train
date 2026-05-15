smell_id:alternative_return_types

# Example 01 - CSV reader with `raw` option changing the return type

```elixir
defmodule CsvReader do
  def read(path, opts \ []) do
    content = File.read!(path)

    case opts[:raw] do
      true ->
        content

      _ ->
        content
        |> String.split("
", trim: true)
        |> Enum.map(&String.split(&1, ","))
    end
  end
end

# Usage
rows = CsvReader.read("users.csv")
# => [["id", "name"], ["1", "Lucas"]]

raw = CsvReader.read("users.csv", raw: true)
# => "id,name\n1,Lucas\n"
```

The `read/2` function returns either a binary or a list of lists depending on an option, which makes its contract hard to reason about.
