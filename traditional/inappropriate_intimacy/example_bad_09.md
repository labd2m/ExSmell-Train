smell_id:inappropriate_intimacy

# Example 09 - Reservation module controlling room policy itself

```elixir
defmodule Room do
  defstruct [:number, :capacity, :pets_allowed, :status]
end

defmodule Reservation do
  def can_book?(%Room{} = room, guests, with_pet?) do
    room.status == :available and
      guests <= room.capacity and
      (not with_pet? or room.pets_allowed)
  end
end
```

The reservation logic is implemented by interrogating `Room` internals instead of encapsulating room booking policy behind the `Room` module.
