smell_id:large_code_generation_by_macros
example_id:04
title: Validation macro that repeats a large changeset pipeline for every field

# Example 04

This looks convenient, but every `required_field/2` and `string_field/2` invocation emits a lot of repeated code. A schema with many fields can explode compile time and generated code size.

```elixir
defmodule FormDSL do
  defmacro __using__(_opts) do
    quote do
      import FormDSL
      Module.register_attribute(__MODULE__, :declared_fields, accumulate: true)
    end
  end

  defmacro required_field(name, opts \\ []) do
    quote do
      field_name = unquote(name)
      opts = unquote(opts)

      if not is_atom(field_name), do: raise ArgumentError, "field name must be an atom"

      @declared_fields {field_name, :required, opts}

      def validate(params, errors) do
        value = Map.get(params, Atom.to_string(field_name)) || Map.get(params, field_name)

        cond do
          is_nil(value) ->
            [{field_name, Keyword.get(opts, :message, "can't be blank")} | errors]

          is_binary(value) and String.trim(value) == "" ->
            [{field_name, Keyword.get(opts, :message, "can't be blank")} | errors]

          true ->
            errors
        end
      end
    end
  end

  defmacro string_field(name, opts \\ []) do
    quote do
      field_name = unquote(name)
      opts = unquote(opts)

      @declared_fields {field_name, :string, opts}

      def cast(params, acc) do
        raw = Map.get(params, Atom.to_string(field_name)) || Map.get(params, field_name)

        casted =
          cond do
            is_binary(raw) -> String.trim(raw)
            is_nil(raw) -> Keyword.get(opts, :default)
            true -> to_string(raw)
          end

        Map.put(acc, field_name, casted)
      end
    end
  end
end

defmodule RegistrationForm do
  use FormDSL

  required_field :email
  required_field :password
  required_field :name
  string_field :email
  string_field :password
  string_field :name
end
```