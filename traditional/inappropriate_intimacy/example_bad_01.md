smell_id:inappropriate_intimacy

# Example 01 - Order reaching into payment details

```elixir
defmodule Gateway do
  def charge(_gateway_name, _card_number, _cvc, _amount) do
    :ok
  end
end

defmodule Card do
  defstruct [:number, :cvc, :status, :gateway_name]
end

defmodule User do
  defstruct [:name, :default_card]
end

defmodule Order do
  def charge(amount, %User{} = user) do
    card = user.default_card

    if card.status == :active do
      Gateway.charge(card.gateway_name, card.number, card.cvc, amount)
    else
      {:error, :inactive_card}
    end
  end
end
```

`Order.charge/2` knows too much about the internal structure of `User` and `Card`, and also about how `Gateway` should be called.
