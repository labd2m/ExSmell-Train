smell_id:accessing_non_existent_map_struct_fields

# Example 02 - API params interpreted through dynamic access

```elixir
defmodule MyAppWeb.InvoiceController do
  def create(params) do
    if params[:discount_code] do
      {:ok, %{mode: :discounted, code: params[:discount_code]}}
    else
      {:ok, %{mode: :regular}}
    end
  end
end

# Use examples
# iex> MyAppWeb.InvoiceController.create(%{discount_code: "BLACKFRIDAY"})
# {:ok, %{mode: :discounted, code: "BLACKFRIDAY"}}
#
# iex> MyAppWeb.InvoiceController.create(%{})
# {:ok, %{mode: :regular}}
#
# iex> MyAppWeb.InvoiceController.create(%{discount_code: nil})
# {:ok, %{mode: :regular}}
```

Here `params[:discount_code]` makes a missing key and an explicitly null-like value look the same. That ambiguity is dangerous at the boundary of a controller.
