smell_id:large_code_generation_by_macros
example_id:09
title: Internationalization DSL that emits one translation function per phrase

# Example 09

This macro generates multiple functions and fallback logic per translation entry. A large translation table can bloat compilation.

```elixir
defmodule I18nDSL do
  defmacro __using__(_opts) do
    quote do
      import I18nDSL
      Module.register_attribute(__MODULE__, :translations, accumulate: true)
      @before_compile I18nDSL
    end
  end

  defmacro phrase(locale, key, template) do
    quote do
      locale = unquote(locale)
      key = unquote(key)
      template = unquote(template)

      if not is_binary(locale), do: raise ArgumentError, "locale must be binary"
      if not is_atom(key), do: raise ArgumentError, "key must be atom"
      if not is_binary(template), do: raise ArgumentError, "template must be binary"

      @translations {locale, key, template}

      def translate(unquote(locale), unquote(key), assigns \\ %{}) do
        rendered =
          Regex.replace(~r/\%\{([a-zA-Z_]+)\}/, unquote(template), fn _, captured ->
            value =
              assigns
              |> Enum.into(%{})
              |> Map.get(String.to_existing_atom(captured), "<?>")

            to_string(value)
          end)

        if String.contains?(rendered, "<?>") do
          send(self(), {:missing_translation_assigns, unquote(locale), unquote(key)})
        end

        rendered
      end
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def translate(_locale, key, _assigns), do: "[missing #{inspect(key)}]"
      def known_translations, do: @translations
    end
  end
end

defmodule Messages do
  use I18nDSL

  phrase "en", :welcome, "Welcome %{name}"
  phrase "en", :invoice_paid, "Invoice %{id} has been paid"
  phrase "pt-BR", :welcome, "Bem-vindo %{name}"
  phrase "pt-BR", :invoice_paid, "Fatura %{id} foi paga"
  phrase "es", :welcome, "Bienvenido %{name}"
end
```