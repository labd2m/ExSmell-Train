smell_id:using_exceptions_for_control_flow

# Example 04

A currency conversion helper raises for unsupported currencies, so the checkout flow uses exception handling to branch into a fallback path.

```elixir
defmodule Fx do
  def to_cents!(amount, "USD") when is_number(amount), do: round(amount * 100)
  def to_cents!(amount, "EUR") when is_number(amount), do: round(amount * 108)
  def to_cents!(_amount, _currency), do: raise ArgumentError, "unsupported currency"
end

defmodule Checkout do
  def normalize_total(amount, currency) do
    try do
      {:ok, Fx.to_cents!(amount, currency)}
    rescue
      e in ArgumentError ->
        {:fallback_to_manual_review, e.message}
    end
  end
end

# Use examples
Checkout.normalize_total(12.50, "USD")
Checkout.normalize_total(12.50, "BRL")
```
