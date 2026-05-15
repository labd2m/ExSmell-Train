smell_id:unrelated_multi_clause_function

# Example 08

This parser-like function actually mixes **address normalization**, **coupon validation**, and **timezone extraction**.

```elixir
defmodule MyApp.Parser do
  @doc """
  Parse incoming values.
  """
  def parse(%{street: street, city: city, zip: zip})
      when is_binary(street) and is_binary(city) and is_binary(zip) do
    %{street: String.trim(street), city: String.upcase(city), zip: zip}
  end

  # unrelated: promotions logic
  def parse(%{coupon: code, total_cents: total}) when is_binary(code) and is_integer(total) do
    if String.starts_with?(code, "PROMO-") and total > 5000 do
      {:ok, 1000}
    else
      {:error, :invalid_coupon}
    end
  end

  # unrelated: user profile/settings logic
  def parse(%{profile: %{timezone: timezone}}) when is_binary(timezone) do
    String.upcase(timezone)
  end
end
```
