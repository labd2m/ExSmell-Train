smell_id:alternative_return_types

# Example 06 - Config loader returning parsed data or raw text

```elixir
defmodule ConfigLoader do
  def load(path, opts \ []) do
    body = File.read!(path)

    case opts[:format] do
      :raw ->
        body

      _ ->
        YamlElixir.read_from_string!(body)
    end
  end
end

# Usage
ConfigLoader.load("config.yml")
# => %{"port" => 4000, "host" => "localhost"}

ConfigLoader.load("config.yml", format: :raw)
# => "port: 4000\nhost: localhost\n"
```

A caller may receive either a parsed map or a binary depending on the option value.
