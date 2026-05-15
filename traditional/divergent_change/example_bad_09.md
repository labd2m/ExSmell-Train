smell_id:divergent_change

# Example 09

```elixir
defmodule IoT.DeviceManager do
  @moduledoc """
  Firmware rollout, alert thresholds, billing quotas,
  and device naming policy are all bundled together.
  """

  # Reason to change: firmware rollout strategy
  def rollout_group(device) do
    if rem(device.id, 10) == 0, do: :canary, else: :stable
  end

  # Reason to change: telemetry alert thresholds
  def high_temperature?(reading) do
    reading.celsius >= 85
  end

  # Reason to change: subscription quota policy
  def allowed_events_per_day(plan) do
    case plan do
      :free -> 1_000
      :pro -> 100_000
      :enterprise -> 1_000_000
    end
  end

  # Reason to change: device naming conventions
  def display_name(device) do
    "#{device.site_code}-#{device.serial}"
  end
end
```
