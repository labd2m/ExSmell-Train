smell_id:large_code_generation_by_macros
example_id:01
title: Router DSL that inlines large validation and dispatch code for every route

# Example 01

This macro expands a large block of code every time `get/3` is used. A module with many routes will generate a lot of repeated code during compilation.

```elixir
defmodule MiniRouter do
  defmacro __using__(_opts) do
    quote do
      import MiniRouter
      Module.register_attribute(__MODULE__, :routes, accumulate: true)
      @before_compile MiniRouter
    end
  end

  defmacro get(path, controller, action) do
    quote do
      path = unquote(path)
      controller = unquote(controller)
      action = unquote(action)

      if not is_binary(path) do
        raise ArgumentError, "path must be a binary"
      end

      if not is_atom(action) do
        raise ArgumentError, "action must be an atom"
      end

      if not is_atom(controller) do
        raise ArgumentError, "controller must be a module alias"
      end

      segments =
        path
        |> String.split("/", trim: true)
        |> Enum.map(fn segment ->
          cond do
            String.starts_with?(segment, ":") ->
              {:param, String.trim_leading(segment, ":")}

            segment == "*" ->
              :wildcard

            true ->
              {:literal, segment}
          end
        end)

      @routes {path, controller, action, segments}

      def dispatch(unquote(path), params, conn) do
        normalized_params =
          params
          |> Enum.map(fn {k, v} -> {to_string(k), v} end)
          |> Enum.into(%{})

        metadata = %{
          route: unquote(path),
          controller: unquote(controller),
          action: unquote(action),
          segment_count: length(unquote(Macro.escape(
            path
            |> String.split("/", trim: true)
            |> Enum.map(& &1)
          )))
        }

        if Map.has_key?(normalized_params, "_trace") do
          send(self(), {:trace_route, metadata})
        end

        conn =
          conn
          |> Map.put(:private, Map.get(conn, :private, %{}))
          |> put_in([:private, :route], metadata)

        apply(unquote(controller), unquote(action), [conn, normalized_params])
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def dispatch(path, _params, _conn) do
        {:error, {:not_found, path}}
      end

      def routes do
        @routes
      end
    end
  end
end

defmodule UserController do
  def index(conn, params), do: {:ok, conn, params}
  def show(conn, params), do: {:ok, conn, params}
end

defmodule AppRouter do
  use MiniRouter

  get "/users", UserController, :index
  get "/users/:id", UserController, :show
  get "/admin/reports/daily", UserController, :index
  get "/admin/reports/monthly", UserController, :index
  get "/admin/reports/yearly", UserController, :index
end
```