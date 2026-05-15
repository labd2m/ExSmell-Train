smell_id:unrelated_multi_clause_function

# Example 05

This `build/1` function assembles completely different artifacts: **SQL queries**, **welcome emails**, and **cache keys**.

```elixir
defmodule MyApp.Builder do
  defmodule SearchQuery do
    defstruct [:table, :term]
  end

  defmodule WelcomeEmail do
    defstruct [:name, :email]
  end

  defmodule CacheKey do
    defstruct [:prefix, :id]
  end

  @doc """
  Build something from input.
  """
  def build(%SearchQuery{table: table, term: term}) when is_binary(table) and is_binary(term) do
    "SELECT * FROM #{table} WHERE name ILIKE '%#{term}%'"
  end

  # unrelated: email content generation
  def build(%WelcomeEmail{name: name, email: email}) when is_binary(name) and is_binary(email) do
    %{to: email, subject: "Welcome #{name}", body: "Hi #{name}, thanks for joining us."}
  end

  # unrelated: cache naming
  def build(%CacheKey{prefix: prefix, id: id}) when is_binary(prefix) do
    "#{prefix}:#{id}"
  end
end
```
