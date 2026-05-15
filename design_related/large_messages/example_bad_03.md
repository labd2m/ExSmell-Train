smell_id:large_messages
example_id:example_bad_03

# Example 03 - Chat server broadcasts the full chat history to each client

```elixir
defmodule ChatClient do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:chat_history, history} ->
        length(history)
        loop()
    end
  end
end

defmodule ChatRoom do
  def broadcast_history(client_pids, history) do
    Enum.each(client_pids, fn pid ->
      # Bad smell: the whole accumulated history is copied to every client
      send(pid, {:chat_history, history})
    end)
  end
end

# Use example:
history =
  Enum.map(1..500_000, fn i ->
    %{user: "user_\#{i}", message: String.duplicate("hello", 20)}
  end)

clients = Enum.map(1..10, fn _ -> ChatClient.start() end)
ChatRoom.broadcast_history(clients, history)
```

Instead of sending only the delta or an identifier, the room pushes a huge history structure to every receiver.
