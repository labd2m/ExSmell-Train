smell_id:large_messages
example_id:example_bad_01

# Example 01 - CSV importer sends all parsed rows to another process

```elixir
defmodule CsvReceiver do
  def start do
    spawn(__MODULE__, :loop, [])
  end

  def loop do
    receive do
      {:import_rows, rows} ->
        # rows may contain hundreds of thousands of parsed CSV rows
        Enum.count(rows)
        loop()
    end
  end
end

defmodule CsvImporter do
  def import(path, receiver_pid) do
    rows =
      path
      |> File.read!()
      |> String.split("\n", trim: true)
      |> Enum.map(&String.split(&1, ","))

    # Bad smell: a huge list of rows is copied to another process
    send(receiver_pid, {:import_rows, rows})
  end
end

# Use example:
receiver = CsvReceiver.start()
CsvImporter.import("customers.csv", receiver)
```

The importer reads and parses the entire file, then sends the full in-memory row list as one large message.
