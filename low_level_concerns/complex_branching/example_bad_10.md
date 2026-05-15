smell_id:complex_branching

# Example 10 — Order cancellation service with too many result variants

```elixir
defmodule MyApp.Orders do
  alias MyApp.{Repo, Order}
  import Ecto.Query

  def cancel_order(order_id, actor_id) do
    case Repo.get(Order, order_id) do
      nil ->
        {:error, :not_found}

      %Order{status: "draft"} = order ->
        case authorize(actor_id, order) do
          :ok ->
            case Repo.delete(order) do
              {:ok, _} -> {:ok, :deleted}
              {:error, %Ecto.Changeset{} = changeset} -> {:error, {:delete_failed, changeset}}
              {:error, reason} -> {:error, {:repo_delete_error, reason}}
            end

          {:error, :forbidden} ->
            {:error, :forbidden}

          {:error, reason} ->
            {:error, {:auth_error, reason}}
        end

      %Order{status: "paid"} = order ->
        case request_refund(order) do
          {:ok, refund_id} ->
            case Repo.update(Ecto.Changeset.change(order, status: "cancelled")) do
              {:ok, cancelled_order} -> {:ok, {:refunded, refund_id, cancelled_order}}
              {:error, changeset} -> {:error, {:refund_done_but_local_update_failed, changeset}}
            end

          {:error, :already_refunded} ->
            {:error, :already_refunded}

          {:error, :gateway_timeout} ->
            {:error, :gateway_timeout}

          {:error, reason} ->
            {:error, {:refund_error, reason}}
        end

      %Order{status: "shipped"} ->
        {:error, :cannot_cancel_shipped}

      %Order{status: "cancelled"} ->
        {:error, :already_cancelled}

      %Order{status: other} ->
        {:error, {:unsupported_status, other}}
    end
  end

  defp authorize(actor_id, %Order{user_id: actor_id}), do: :ok
  defp authorize(_, _), do: {:error, :forbidden}

  defp request_refund(%Order{id: id}), do: {:ok, "refund_#{id}"}
end
```

This is a bad example because `cancel_order/2` centralizes state branching, authorization branching, refund branching, and repository branching, making the logic difficult to reason about.
