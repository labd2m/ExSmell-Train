smell_id:large_code_generation_by_macros
example_id:10
title: API client DSL that generates full request preparation and response handling per endpoint

# Example 10

Every `endpoint/4` invocation emits a big request pipeline. With many endpoints, the compiled module becomes large and repetitive.

```elixir
defmodule ClientDSL do
  defmacro __using__(_opts) do
    quote do
      import ClientDSL
      Module.register_attribute(__MODULE__, :endpoints, accumulate: true)
      @before_compile ClientDSL
    end
  end

  defmacro endpoint(fun_name, method, path, opts \\ []) do
    quote do
      fun_name = unquote(fun_name)
      method = unquote(method)
      path = unquote(path)
      opts = unquote(opts)

      if not is_atom(fun_name), do: raise ArgumentError, "function name must be atom"
      if method not in [:get, :post, :put, :delete], do: raise ArgumentError, "invalid method"
      if not is_binary(path), do: raise ArgumentError, "path must be binary"

      @endpoints {fun_name, method, path, opts}

      def unquote(fun_name)(client, params \\ %{}) do
        headers =
          client
          |> Map.get(:headers, [])
          |> Enum.into([])
          |> Kernel.++(Keyword.get(opts, :headers, []))

        payload =
          params
          |> Enum.into(%{})
          |> Enum.reject(fn {_k, v} -> is_nil(v) end)
          |> Enum.into(%{})

        request = %{
          method: unquote(method),
          url: client.base_url <> unquote(path),
          headers: headers,
          timeout: Keyword.get(unquote(opts), :timeout, 5_000),
          payload: payload
        }

        if Map.has_key?(payload, :debug) do
          send(self(), {:debug_request, request})
        end

        case client.adapter.request(request) do
          {:ok, %{status: status, body: body}} when status in 200..299 ->
            {:ok, %{endpoint: unquote(fun_name), body: body}}

          {:ok, %{status: status, body: body}} ->
            {:error, %{endpoint: unquote(fun_name), status: status, body: body}}

          other ->
            other
        end
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def endpoints, do: @endpoints
    end
  end
end

defmodule BillingClient do
  use ClientDSL

  endpoint :get_customer, :get, "/customers/:id", timeout: 2_000
  endpoint :create_invoice, :post, "/invoices", headers: [{"content-type", "application/json"}]
  endpoint :pay_invoice, :post, "/invoices/:id/pay"
  endpoint :refund_invoice, :post, "/invoices/:id/refund"
  endpoint :list_invoices, :get, "/invoices"
end
```