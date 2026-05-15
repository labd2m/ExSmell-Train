smell_id:accessing_non_existent_map_struct_fields

# Example 07 - Payment gateway response handling

```elixir
defmodule PaymentGateway do
  def normalize(response) do
    %{
      status: response[:status],
      auth_code: response[:auth_code],
      fraud_score: response[:fraud_score]
    }
  end
end

# Use examples
# iex> PaymentGateway.normalize(%{status: :approved, auth_code: "OK123"})
# %{status: :approved, auth_code: "OK123", fraud_score: nil}
#
# iex> PaymentGateway.normalize(%{status: :approved, auth_code: "OK123", fraud_score: nil})
# %{status: :approved, auth_code: "OK123", fraud_score: nil}
```

The normalized output makes a missing fraud score indistinguishable from an explicitly present `nil` score.
