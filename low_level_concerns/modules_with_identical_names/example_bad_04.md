smell_id:modules_with_identical_names

# Example 04

A real module and a test support module use the same name. Running tests may redefine the production module in memory.

## lib/my_app/mailer.ex
```elixir
defmodule MyApp.Mailer do
  def deliver(email) do
    {:sent, email}
  end
end
```

## test/support/mailer.ex
```elixir
defmodule MyApp.Mailer do
  def deliver(email) do
    send(self(), {:fake_delivery, email})
    :ok
  end
end
```

## Production caller
```elixir
defmodule MyApp.Notifications do
  def welcome(user) do
    MyApp.Mailer.deliver(%{to: user.email, subject: "Welcome"})
  end
end
```

## Why this smells
The test helper did not get a different name such as `MyApp.MailerFake`. Instead, it reuses `MyApp.Mailer`, creating a module name collision.
