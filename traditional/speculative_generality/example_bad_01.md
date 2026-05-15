smell_id:speculative_generality

# Example 01

```elixir
defmodule ShoppingCart do
  def apply_discount(%{category: category} = product) do
    case category do
      :electronics -> product.price * 0.8
      :books -> product.price * 0.8
      :toys -> product.price * 0.8
      _ -> product.price * 0.8
    end
  end
end
```

The code pretends discounts will vary by `category`, but every branch applies the same rule.
