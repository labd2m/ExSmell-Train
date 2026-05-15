smell_id:long_function

```elixir
defmodule ProductPresenter do
  def render(product, locale, currency_symbol) do
    title =
      case locale do
        "en" -> product.name_en
        "pt" -> product.name_pt
        "es" -> product.name_es
        _ -> product.name_en
      end

    description =
      case locale do
        "en" -> product.description_en
        "pt" -> product.description_pt
        "es" -> product.description_es
        _ -> product.description_en
      end

    price = product.price_cents / 100
    stock_label = if product.stock > 0, do: "In stock", else: "Out of stock"
    tags = product.tags |> Enum.map(&String.upcase/1) |> Enum.join(", ")
    image_block = Enum.map(product.images, fn img -> "<img src='#{img}' />" end) |> Enum.join("\n")

    html = """
    <section>
      <h1>#{title}</h1>
      <p>#{description}</p>
      <p>#{currency_symbol}#{price}</p>
      <p>#{stock_label}</p>
      <p>#{tags}</p>
      #{image_block}
    </section>
    """

    IO.puts("Rendered product #{product.id}")
    html
  end
end
```
