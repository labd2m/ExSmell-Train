id:uacl_06
smell_id:using_app_configuration_for_libraries

# Example 06 - Pagination helper locked to one global page size

```elixir
# config/config.exs
import Config

config :pagination_lib,
  page_size: 25
```

```elixir
defmodule PaginationLib do
  def paginate(list, page) when is_list(list) and is_integer(page) and page > 0 do
    page_size = Application.fetch_env!(:pagination_lib, :page_size)
    offset = (page - 1) * page_size

    Enum.slice(list, offset, page_size)
  end
end
```

```elixir
defmodule ProductsPage do
  def list(items, page), do: PaginationLib.paginate(items, page)
end

defmodule AuditEventsPage do
  def list(events, page), do: PaginationLib.paginate(events, page)
end
```

```elixir
iex> PaginationLib.paginate(Enum.to_list(1..100), 1)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25]
```

The helper acts like a reusable library, but all callers are forced to reuse the
same page size defined at application level.
