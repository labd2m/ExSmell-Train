smell_id:working_with_invalid_data
# Example 10 — Background job forwards invalid archive data to `:zlib`

This final example shows the smell outside local scope.  
A background job receives arbitrary job arguments, passes them to an archiver module, and only then does the low-level compression library fail.

## Bad code

```elixir
defmodule MyApp.CleanupJob do
  alias MyApp.Archiver

  def perform(%{"payload" => payload}) do
    Archiver.compress!(payload)
  end
end

defmodule MyApp.Archiver do
  def compress!(payload) do
    # expected:
    #   payload -> binary or iodata
    #
    # bad smell:
    #   assumes job args already have the correct type
    :zlib.gzip(payload)
  end
end
```

## Example usage

```elixir
# valid job args
%{"payload" => "temporary-report-content"}

# invalid job args
%{"payload" => %{report_id: 10, format: "csv"}}

# representative failure
** (ArgumentError) argument error
    :zlib.gzip(%{report_id: 10, format: "csv"})
```

## Why this is a smell

The asynchronous job boundary never validates the shape or type of the incoming payload.
As a result, the error emerges in a low-level Erlang module, which makes troubleshooting slower and less direct.
