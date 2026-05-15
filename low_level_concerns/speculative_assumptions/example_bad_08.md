smell_id:speculative_assumptions

# Example 08 - Version parser that quietly misreads richer formats

```elixir
defmodule ReleaseVersion do
  @doc """
  Returns the minor version number.

  The implementation assumes versions always look like `major.minor.patch`.
  """
  def minor(version) do
    version
    |> String.split(".")
    |> Enum.at(1)
  end
end

# Planned input
ReleaseVersion.minor("2.14.3")
# => "14"

# Unplanned input returns the wrong piece instead of failing fast
ReleaseVersion.minor("2.beta.14.3")
# => "beta"
```
