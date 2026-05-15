smell_id:alternative_return_types

# Example 07 - Image thumbnail builder returning a file path or binary data

```elixir
defmodule Thumbnailer do
  def build(image_path, opts \ []) do
    thumb = ThirdPartyImageLib.thumbnail!(image_path, 200, 200)

    case opts[:output] do
      :binary ->
        File.read!(thumb)

      _ ->
        thumb
    end
  end
end

# Usage
Thumbnailer.build("avatar.png")
# => "/tmp/avatar_thumb.png"

Thumbnailer.build("avatar.png", output: :binary)
# => <<137, 80, 78, 71, ...>>
```

The same public API returns either a path string or raw binary content.
