smell_id:agent_obsession

# Example 10 — Workflow state spread across approval, payment, and shipping modules

```elixir
defmodule Orders.WorkflowState do
  def start_link(order_id) do
    Agent.start_link(fn ->
      %{order_id: order_id, approved: false, paid: false, shipped: false, notes: []}
    end)
  end
end

defmodule Orders.Approval do
  def approve(workflow_pid, approver) do
    Agent.update(workflow_pid, fn state ->
      %{state | approved: true, notes: ["approved by #{approver}" | state.notes]}
    end)
  end
end

defmodule Orders.Payment do
  def capture(workflow_pid, transaction_id) do
    Agent.update(workflow_pid, fn state ->
      %{state | paid: true, notes: ["payment #{transaction_id}" | state.notes]}
    end)
  end
end

defmodule Orders.Shipping do
  def mark_shipped(workflow_pid, tracking_code) do
    Agent.update(workflow_pid, fn state ->
      %{state | shipped: true, notes: ["tracking #{tracking_code}" | state.notes]}
    end)
  end
end

defmodule Orders.WorkflowView do
  def read(workflow_pid) do
    Agent.get(workflow_pid, & &1)
  end
end
```

This workflow behaves like a custom server but without a single owner module. The protocol is smeared across several modules, each directly reaching into the `Agent`.
