smell_id:alternative_return_types

# Example 08 - Job runner returning a `Task` or the actual result

```elixir
defmodule MyApp.JobRunner do
  def run(fun, opts \ []) when is_function(fun, 0) do
    case opts[:async] do
      true ->
        Task.async(fun)

      _ ->
        {:ok, fun.()}
    end
  end
end

# Usage
MyApp.JobRunner.run(fn -> 42 end)
# => {:ok, 42}

MyApp.JobRunner.run(fn -> 42 end, async: true)
# => %Task{owner: #PID<0.123.0>, pid: #PID<0.124.0>, ref: ...}
```

The `:async` option changes the return from a result tuple to a `Task` struct, which is a very different contract.
