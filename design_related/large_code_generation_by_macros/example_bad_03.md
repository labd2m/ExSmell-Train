smell_id:large_code_generation_by_macros
example_id:03
title: Event DSL that expands a full parser and handler wrapper for each event

# Example 03

The `event/3` macro inlines validation, decoding, telemetry, and dispatch logic every time it is called.

```elixir
defmodule EventDSL do
  defmacro __using__(_opts) do
    quote do
      import EventDSL
      Module.register_attribute(__MODULE__, :events, accumulate: true)
      @before_compile EventDSL
    end
  end

  defmacro event(name, version, handler) do
    quote do
      name = unquote(name)
      version = unquote(version)
      handler = unquote(handler)

      if not is_binary(name), do: raise ArgumentError, "event name must be a binary"
      if not is_integer(version), do: raise ArgumentError, "version must be an integer"
      if not is_atom(handler), do: raise ArgumentError, "handler must be a module"

      descriptor = %{name: name, version: version, handler: handler}
      @events descriptor

      def handle_event(%{"name" => unquote(name), "version" => unquote(version)} = raw) do
        payload =
          raw
          |> Map.get("payload", %{})
          |> Enum.reduce(%{}, fn {k, v}, acc ->
            Map.put(acc, to_string(k), v)
          end)

        metadata = %{
          correlation_id: Map.get(raw, "correlation_id"),
          producer: Map.get(raw, "producer"),
          received_at: System.system_time(:microsecond)
        }

        if map_size(payload) > 100 do
          send(self(), {:large_payload_received, unquote(name)})
        end

        apply(unquote(handler), :handle, [%{
          name: unquote(name),
          version: unquote(version),
          payload: payload,
          metadata: metadata
        }])
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def handle_event(event), do: {:error, {:unknown_event, event}}
      def supported_events, do: @events
    end
  end
end

defmodule UserCreatedHandler do
  def handle(event), do: {:ok, event}
end

defmodule InvoicePaidHandler do
  def handle(event), do: {:ok, event}
end

defmodule Consumer do
  use EventDSL

  event "user.created", 1, UserCreatedHandler
  event "invoice.paid", 1, InvoicePaidHandler
  event "invoice.paid", 2, InvoicePaidHandler
  event "report.generated", 1, InvoicePaidHandler
end
```