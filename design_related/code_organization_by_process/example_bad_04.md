smell_id:code_organization_by_process

# Example 04 - Validation service implemented as a GenServer

```elixir
defmodule SignupRules do
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, :ok)
  end

  def valid_email?(pid, email) do
    GenServer.call(pid, {:valid_email?, email})
  end

  def strong_password?(pid, password) do
    GenServer.call(pid, {:strong_password?, password})
  end

  def init(:ok), do: {:ok, nil}

  def handle_call({:valid_email?, email}, _from, state) do
    {:reply, String.contains?(email, "@"), state}
  end

  def handle_call({:strong_password?, password}, _from, state) do
    result = String.length(password) >= 12 and String.match?(password, ~r/[0-9]/)
    {:reply, result, state}
  end
end

{:ok, pid} = SignupRules.start_link()
SignupRules.valid_email?(pid, "ana@example.com")
SignupRules.strong_password?(pid, "Secret123456")
```
