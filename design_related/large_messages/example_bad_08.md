smell_id:large_messages
example_id:example_bad_08

# Example 08 - Background task sends a massive JSON-decoded payload

```elixir
defmodule ApiConsumer do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:api_payload, payload} ->
        payload["items"] |> length()
        loop()
    end
  end
end

defmodule ApiSync do
  def handoff(response_body, consumer_pid) do
    payload = Jason.decode!(response_body)

    # Bad smell: a huge decoded map/list structure is copied to another process
    send(consumer_pid, {:api_payload, payload})
  end
end

# Use example:
response_body =
  Jason.encode!(%{
    "items" =>
      Enum.map(1..250_000, fn i ->
        %{"id" => i, "name" => "item_\#{i}", "attrs" => Enum.to_list(1..20)}
      end)
  })

consumer = ApiConsumer.start()
ApiSync.handoff(response_body, consumer)
```

The decoded response may be extremely large, and passing it whole between processes amplifies copying costs.
