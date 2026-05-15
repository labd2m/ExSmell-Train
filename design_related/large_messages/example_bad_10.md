smell_id:large_messages
example_id:example_bad_10

# Example 10 - Search indexer receives the full document corpus

```elixir
defmodule IndexWorker do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:reindex, documents} ->
        length(documents)
        loop()
    end
  end
end

defmodule SearchRebuilder do
  def rebuild(index_pid) do
    documents =
      Enum.map(1..400_000, fn i ->
        %{
          id: i,
          title: "Document \#{i}",
          body: String.duplicate("elixir search indexing ", 50),
          metadata: %{tags: Enum.map(1..10, &"tag_\#{&1}")}
        }
      end)

    # Bad smell: the whole corpus is copied in a process message
    send(index_pid, {:reindex, documents})
  end
end

# Use example:
indexer = IndexWorker.start()
SearchRebuilder.rebuild(indexer)
```

The reindex operation materializes and transfers the entire corpus as one huge message, which is expensive to copy.
