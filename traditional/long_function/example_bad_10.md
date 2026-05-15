smell_id:long_function

```elixir
defmodule InventorySync do
  def sync(products, warehouse_client, repo) do
    report =
      Enum.reduce(products, %{ok: 0, failed: 0, skipped: 0}, fn product, acc ->
        if product.discontinued do
          IO.puts("Skipping discontinued product #{product.sku}")
          %{acc | skipped: acc.skipped + 1}
        else
          payload = %{
            sku: product.sku,
            stock: product.stock,
            reserved: product.reserved,
            available: product.stock - product.reserved,
            price: product.price
          }

          case warehouse_client.push(payload) do
            {:ok, remote_id} ->
              repo.update_last_sync(product.id, remote_id, DateTime.utc_now())
              IO.puts("Synced #{product.sku}")
              %{acc | ok: acc.ok + 1}

            {:error, :timeout} ->
              repo.insert_sync_error(product.id, :timeout)
              IO.puts("Timeout syncing #{product.sku}")
              %{acc | failed: acc.failed + 1}

            {:error, reason} ->
              repo.insert_sync_error(product.id, reason)
              IO.puts("Failed syncing #{product.sku}: #{inspect(reason)}")
              %{acc | failed: acc.failed + 1}
          end
        end
      end)

    IO.puts("Sync report: #{inspect(report)}")
    report
  end
end
```
