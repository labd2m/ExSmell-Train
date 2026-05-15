smell_id:dynamic_atom_creation

# Example 5

```elixir
defmodule MyApp.JobBuilder do
  def enqueue(worker_name, args) do
    worker_module =
      worker_name
      |> String.to_atom()

    %{worker: worker_module, args: args}
  end
end

# Example:
# MyApp.JobBuilder.enqueue("Elixir.MyApp.Workers.SendEmail", %{id: 1})
```

The worker name is provided dynamically and converted into an atom every time new input appears.
