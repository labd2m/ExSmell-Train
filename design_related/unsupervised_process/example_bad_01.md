smell_id:unsupervised_process

# Example 01 - Per-user cart processes started directly from a controller

```elixir
defmodule Shop.Cart do
  use GenServer

  def start(user_id) do
    GenServer.start(__MODULE__, %{user_id: user_id, items: []})
  end

  def add_item(pid, sku) do
    GenServer.cast(pid, {:add_item, sku})
  end

  @impl true
  def init(state), do: {:ok, state}

  @impl true
  def handle_cast({:add_item, sku}, state) do
    {:noreply, %{state | items: [sku | state.items]}}
  end
end

defmodule ShopWeb.CartController do
  def create_cart(conn, %{"user_id" => user_id}) do
    # A new long-running process is started for each request.
    # None of these cart processes belong to a supervision tree.
    {:ok, pid} = Shop.Cart.start(user_id)

    Shop.Cart.add_item(pid, "starter-pack")
    send_resp(conn, 201, "cart created")
  end

  defp send_resp(conn, _status, _body), do: conn
end
```
