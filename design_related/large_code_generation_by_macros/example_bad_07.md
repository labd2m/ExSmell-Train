smell_id:large_code_generation_by_macros
example_id:07
title: Metrics DSL that expands whole timing and tagging wrappers for each metric

# Example 07

Instead of a small reusable runtime function, the macro duplicates the whole wrapper logic for every metric declaration.

```elixir
defmodule MetricsDSL do
  defmacro __using__(_opts) do
    quote do
      import MetricsDSL
      Module.register_attribute(__MODULE__, :metric_defs, accumulate: true)
      @before_compile MetricsDSL
    end
  end

  defmacro timed(metric_name, event_name) do
    quote do
      metric_name = unquote(metric_name)
      event_name = unquote(event_name)

      @metric_defs {metric_name, event_name}

      def run_timed(unquote(metric_name), fun, metadata \\ %{}) when is_function(fun, 0) do
        start = System.monotonic_time()
        result = fun.()
        stop = System.monotonic_time()

        duration = System.convert_time_unit(stop - start, :native, :microsecond)

        normalized_metadata =
          metadata
          |> Enum.into(%{})
          |> Map.put(:metric, metric_name)
          |> Map.put(:event, event_name)
          |> Map.put(:duration_us, duration)

        send(self(), {:metric_recorded, normalized_metadata})

        {:ok, %{result: result, duration_us: duration}}
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def run_timed(_name, fun, _metadata) when is_function(fun, 0), do: {:ok, %{result: fun.()}}
      def metrics, do: @metric_defs
    end
  end
end

defmodule AppMetrics do
  use MetricsDSL

  timed :user_lookup, "users.lookup"
  timed :invoice_generation, "invoices.generate"
  timed :email_delivery, "emails.deliver"
  timed :report_export, "reports.export"
  timed :image_resize, "images.resize"
end
```