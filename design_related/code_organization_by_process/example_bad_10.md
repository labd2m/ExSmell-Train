smell_id:code_organization_by_process

# Example 10 - Encryption helpers hidden behind a server API

```elixir
defmodule TokenCodec do
  use GenServer

  @secret "my-secret"

  def start_link do
    GenServer.start_link(__MODULE__, :ok)
  end

  def sign(pid, value) do
    GenServer.call(pid, {:sign, value})
  end

  def verify(pid, signed) do
    GenServer.call(pid, {:verify, signed})
  end

  def init(:ok), do: {:ok, nil}

  def handle_call({:sign, value}, _from, state) do
    payload = Base.encode64(value)
    {:reply, payload <> "." <> @secret, state}
  end

  def handle_call({:verify, signed}, _from, state) do
    result = String.ends_with?(signed, "." <> @secret)
    {:reply, result, state}
  end
end

{:ok, pid} = TokenCodec.start_link()
encoded = TokenCodec.sign(pid, "user:10")
TokenCodec.verify(pid, encoded)
```
