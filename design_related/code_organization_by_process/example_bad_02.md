smell_id:code_organization_by_process

# Example 02 - String formatting pushed through a server

```elixir
defmodule TextFormatter do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, %{})
  end

  def snake_to_title(pid, text) do
    GenServer.call(pid, {:snake_to_title, text})
  end

  def truncate(pid, text, size) do
    GenServer.call(pid, {:truncate, text, size})
  end

  def init(state), do: {:ok, state}

  def handle_call({:snake_to_title, text}, _from, state) do
    result =
      text
      |> String.split("_")
      |> Enum.map(&String.capitalize/1)
      |> Enum.join(" ")

    {:reply, result, state}
  end

  def handle_call({:truncate, text, size}, _from, state) do
    {:reply, String.slice(text, 0, size), state}
  end
end

{:ok, pid} = TextFormatter.start_link()
TextFormatter.snake_to_title(pid, "monthly_sales_report")
TextFormatter.truncate(pid, "abcdefghij", 4)
```
