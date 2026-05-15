smell_id:feature_envy

# Example 02

```elixir
defmodule CustomerProfile do
  def get(id) do
    %{
      id: id,
      first_name: "Lucas",
      last_name: "Vegi",
      city: "Belo Horizonte",
      country: "Brazil",
      vip: true
    }
  end

  def vip_badge(true), do: "VIP"
  def vip_badge(false), do: "REGULAR"
end

defmodule CustomerMailer do
  def welcome_message(customer_id) do
    profile = CustomerProfile.get(customer_id)

    full_name = profile.first_name <> " " <> profile.last_name
    location = profile.city <> ", " <> profile.country
    badge = CustomerProfile.vip_badge(profile.vip)

    "Hello #{full_name} from #{location}! Status: #{badge}."
  end
end
```
