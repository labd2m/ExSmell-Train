smell_id:large_code_generation_by_macros
example_id:02
title: Permissions DSL that generates many functions and checks per declaration

# Example 02

Each call to `allow/3` expands several validations, helper functions, and policy clauses. A system with many roles and resources can compile a huge amount of generated code.

```elixir
defmodule PermissionDSL do
  defmacro __using__(_opts) do
    quote do
      import PermissionDSL
      Module.register_attribute(__MODULE__, :rules, accumulate: true)
      @before_compile PermissionDSL
    end
  end

  defmacro allow(role, resource, actions) do
    quote do
      role = unquote(role)
      resource = unquote(resource)
      actions = List.wrap(unquote(actions))

      if not is_atom(role), do: raise ArgumentError, "role must be an atom"
      if not is_atom(resource), do: raise ArgumentError, "resource must be an atom"

      Enum.each(actions, fn action ->
        if not is_atom(action) do
          raise ArgumentError, "all actions must be atoms"
        end
      end)

      normalized_rule = %{
        role: role,
        resource: resource,
        actions: Enum.uniq(actions),
        id: "#{role}:#{resource}:#{Enum.join(Enum.map(actions, &Atom.to_string/1), ",")}"
      }

      @rules normalized_rule

      Enum.each(actions, fn action ->
        def can?(%{role: unquote(role)}, unquote(resource), unquote(action)) do
          audit = %{
            role: unquote(role),
            resource: unquote(resource),
            action: unquote(action),
            at: System.system_time(:millisecond)
          }

          send(self(), {:permission_checked, audit})
          true
        end

        def explain(unquote(role), unquote(resource), unquote(action)) do
          %{
            allowed: true,
            because: "#{unquote(role)} can #{unquote(action)} #{unquote(resource)}"
          }
        end
      end)
    end
  end

  defmacro __before_compile__(_env) do
    quote do
      def can?(_user, _resource, _action), do: false

      def explain(role, resource, action) do
        %{
          allowed: false,
          because: "#{inspect(role)} cannot #{inspect(action)} #{inspect(resource)}"
        }
      end

      def rules, do: @rules
    end
  end
end

defmodule BlogPolicy do
  use PermissionDSL

  allow :admin, :post, [:read, :create, :update, :delete]
  allow :editor, :post, [:read, :create, :update]
  allow :reviewer, :comment, [:read, :delete]
  allow :guest, :post, [:read]
  allow :guest, :comment, [:read]
end
```