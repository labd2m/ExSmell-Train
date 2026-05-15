smell_id:alternative_return_types

# Example 10 - Email normalizer returning a string or structured parts

```elixir
defmodule EmailNormalizer do
  def normalize(email, opts \ []) do
    normalized = email |> String.trim() |> String.downcase()
    [local, domain] = String.split(normalized, "@")

    case opts[:parts] do
      true ->
        %{local: local, domain: domain}

      _ ->
        normalized
    end
  end
end

# Usage
EmailNormalizer.normalize("  Lucas@Example.COM ")
# => "lucas@example.com"

EmailNormalizer.normalize("  Lucas@Example.COM ", parts: true)
# => %{local: "lucas", domain: "example.com"}
```

The return type changes from a normalized string to a map when `:parts` is enabled.
