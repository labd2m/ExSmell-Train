smell_id:switch_statements

# Example 10

```elixir
defmodule OrderStateMachine do
  def can_cancel?(:created), do: true
  def can_cancel?(:paid), do: true
  def can_cancel?(:shipped), do: false
  def can_cancel?(:delivered), do: false
end

defmodule OrderStateLabels do
  def label(:created), do: "Created"
  def label(:paid), do: "Paid"
  def label(:shipped), do: "Shipped"
  def label(:delivered), do: "Delivered"
end

defmodule OrderStateTransitions do
  def next(:created), do: :paid
  def next(:paid), do: :shipped
  def next(:shipped), do: :delivered
  def next(:delivered), do: :delivered
end

defmodule OrderPresenter do
  def summary(order) do
    %{state: state} = order

    %{
      label: OrderStateLabels.label(state),
      can_cancel?: OrderStateMachine.can_cancel?(state),
      next_state: OrderStateTransitions.next(state)
    }
  end
end
```

The order state decision logic is repeated in multiple modules. A new state such as `:returned` would trigger scattered updates.
