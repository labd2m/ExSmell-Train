smell_id:divergent_change

# Example 04

```elixir
defmodule Media.Asset do
  @moduledoc """
  This module mixes image processing, CDN addressing,
  and billing usage calculations.
  """

  # Reason to change: image processing logic
  def thumbnail_path(file_name) do
    "thumb_" <> Path.rootname(file_name) <> ".jpg"
  end

  # Reason to change: CDN URL policy
  def public_url(asset_id, variant) do
    base = Application.fetch_env!(:my_app, :cdn_base_url)
    "#{base}/assets/#{asset_id}/#{variant}"
  end

  # Reason to change: billing / quota policy
  def billable_bytes(asset) do
    if asset.mime_type in ["video/mp4", "video/mov"] do
      asset.bytes * 2
    else
      asset.bytes
    end
  end

  # Reason to change: retention rules
  def purge_after_days(asset) do
    if asset.plan == :enterprise, do: 365, else: 30
  end
end
```
