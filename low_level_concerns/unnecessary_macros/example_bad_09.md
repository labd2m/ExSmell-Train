smell_id:unnecessary_macros

# Example 09

This macro is used in application code, but it only calls a regular library function. The macro adds cognitive overhead without solving a macro-level problem.

```elixir
defmodule MyApp.DateHelpers do
  defmacro current_year do
    quote do
      Date.utc_today().year
    end
  end
end

defmodule MyApp.Report do
  require MyApp.DateHelpers

  def build_title do
    "Annual report - #{MyApp.DateHelpers.current_year()}"
  end
end
```
