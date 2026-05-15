smell_id:large_code_generation_by_macros
example_id:08
title: State machine DSL that expands many transition clauses and validation helpers

# Example 08

This DSL expands whole transition logic for each `transition/3` call. With many states and events, the compiled module grows quickly.

```elixir
defmodule StateMachineDSL do
  defmacro __using__(_opts) do
    quote do
      import StateMachineDSL
      Module.register_attribute(__MODULE__, :transitions, accumulate: true)
      @before_compile StateMachineDSL
    end
  end

  defmacro transition(from, event, to) do
    quote do
      from = unquote(from)
      event = unquote(event)
      to = unquote(to)

      if not is_atom(from), do: raise ArgumentError, "from must be atom"
      if not is_atom(event), do: raise ArgumentError, "event must be atom"
      if not is_atom(to), do: raise ArgumentError, "to must be atom"

      @transitions {from, event, to}

      def apply_transition(%{state: unquote(from)} = aggregate, unquote(event), metadata) do
        next =
          aggregate
          |> Map.put(:state, unquote(to))
          |> Map.update(:history, [unquote(event)], fn history -> [unquote(event) | history] end)
          |> Map.put(:last_transition_metadata, Enum.into(metadata, %{}))

        send(self(), {:transition_applied, unquote(from), unquote(event), unquote(to)})
        {:ok, next}
      end

      def transition_allowed?(%{state: unquote(from)}, unquote(event)) do
        true
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def apply_transition(aggregate, event, _metadata), do: {:error, {:invalid_transition, aggregate.state, event}}
      def transition_allowed?(_aggregate, _event), do: false
      def transitions, do: @transitions
    end
  end
end

defmodule OrderStateMachine do
  use StateMachineDSL

  transition :draft, :submit, :submitted
  transition :submitted, :pay, :paid
  transition :paid, :ship, :shipped
  transition :shipped, :deliver, :delivered
  transition :submitted, :cancel, :cancelled
  transition :paid, :refund, :refunded
end
```