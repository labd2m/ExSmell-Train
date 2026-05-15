smell_id:divergent_change

# Example 10

```elixir
defmodule Marketplace.Seller do
  @moduledoc """
  Payouts, SEO, support automation, and listing review rules
  are unrelated responsibilities living in one module.
  """

  # Reason to change: payout schedule policy
  def next_payout_date(%{country: "BR"}, today), do: Date.add(today, 14)
  def next_payout_date(_seller, today), do: Date.add(today, 7)

  # Reason to change: SEO slug generation
  def listing_slug(title) do
    title
    |> String.downcase()
    |> String.replace(~r/[^a-z0-9\s-]/u, "")
    |> String.replace(" ", "-")
  end

  # Reason to change: support automation policy
  def auto_reply(topic) do
    case topic do
      :refund -> "We received your refund request."
      :payout -> "Your payout is being reviewed."
      _ -> "We will get back to you soon."
    end
  end

  # Reason to change: marketplace compliance rules
  def requires_manual_review?(listing) do
    listing.price > 5_000 or listing.category in ["jewelry", "medical"]
  end
end
```
