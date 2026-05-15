smell_id:code_organization_by_process

# Example 08 - HTTP parameter normalization in a process boundary

```elixir
defmodule ParamsNormalizer do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, :ok)
  end

  def normalize_page(pid, params) do
    GenServer.call(pid, {:normalize_page, params})
  end

  def normalize_sort(pid, params) do
    GenServer.call(pid, {:normalize_sort, params})
  end

  def init(:ok), do: {:ok, %{}}

  def handle_call({:normalize_page, params}, _from, state) do
    page = params |> Map.get("page", "1") |> String.to_integer()
    {:reply, page, state}
  end

  def handle_call({:normalize_sort, params}, _from, state) do
    {:reply, Map.get(params, "sort", "name"), state}
  end
end

defmodule ProductsController do
  def index(params, pid) do
    page = ParamsNormalizer.normalize_page(pid, params)
    sort = ParamsNormalizer.normalize_sort(pid, params)
    %{page: page, sort: sort}
  end
end

{:ok, pid} = ParamsNormalizer.start_link()
ProductsController.index(%{"page" => "2"}, pid)
```
