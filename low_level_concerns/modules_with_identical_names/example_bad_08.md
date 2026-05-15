smell_id:modules_with_identical_names

# Example 08

A nested module is duplicated in separate files, each exposing different functions.

## lib/store/order/status.ex
```elixir
defmodule Store.Order.Status do
  def pending?, do: true
  def states, do: [:pending, :paid]
end
```

## lib/store/order/status_override.ex
```elixir
defmodule Store.Order.Status do
  def cancelled?, do: true
  def states, do: [:cancelled, :refunded]
end
```

## Use example
```elixir
iex> c("lib/store/order/status.ex")
[Store.Order.Status]

iex> Store.Order.Status.pending?()
true

iex> c("lib/store/order/status_override.ex")
warning: redefining module Store.Order.Status (current version defined in memory)
lib/store/order/status_override.ex:1
[Store.Order.Status]

iex> Store.Order.Status.cancelled?()
true

iex> Store.Order.Status.pending?()
** (UndefinedFunctionError) function Store.Order.Status.pending?/0 is undefined or private
```
