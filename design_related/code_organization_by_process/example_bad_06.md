smell_id:code_organization_by_process

# Example 06 - Shopping cart math organized around an Agent

```elixir
defmodule CartMath do
  @doc """
  The Agent stores no meaningful state.
  It only exists so callers can ask it to run pure functions.
  """
  def start_link do
    Agent.start_link(fn -> :idle end)
  end

  def total(pid, items) do
    Agent.get(pid, fn _ -> Enum.reduce(items, 0, fn item, acc -> acc + item.price * item.qty end) end)
  end

  def item_count(pid, items) do
    Agent.get(pid, fn _ -> Enum.reduce(items, 0, fn item, acc -> acc + item.qty end) end)
  end
end

items = [%{price: 10, qty: 2}, %{price: 15, qty: 1}]
{:ok, pid} = CartMath.start_link()
CartMath.total(pid, items)
CartMath.item_count(pid, items)
```
