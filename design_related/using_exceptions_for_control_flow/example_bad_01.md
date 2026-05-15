smell_id:using_exceptions_for_control_flow

# Example 01

A parsing library raises on invalid user input, so the client is forced to use `try/rescue` for an expected branch.

```elixir
defmodule ParseKit do
  def parse_age(value) do
    case Integer.parse(value) do
      {age, ""} when age >= 0 ->
        age

      _ ->
        raise ArgumentError, "age must be a non-negative integer string"
    end
  end
end

defmodule RegistrationController do
  def create(params) do
    try do
      age = ParseKit.parse_age(params["age"])
      {:ok, %{status: :created, age: age}}
    rescue
      e in ArgumentError ->
        {:error, %{status: :bad_request, reason: e.message}}
    end
  end
end

# Use examples
RegistrationController.create(%{"age" => "21"})
RegistrationController.create(%{"age" => "twenty"})
```
