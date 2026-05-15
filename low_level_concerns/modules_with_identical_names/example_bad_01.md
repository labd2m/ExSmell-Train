smell_id:modules_with_identical_names

# Example 01

Two source files define the same top-level module name. Loading the second one replaces the first one in the BEAM.

## module_one.ex
```elixir
defmodule Billing.Reporter do
  @moduledoc "Defined in module_one.ex"

  def source do
    :billing
  end

  def total do
    120
  end
end
```

## module_two.ex
```elixir
defmodule Billing.Reporter do
  @moduledoc "Defined in module_two.ex"

  def source do
    :analytics
  end

  def average do
    15
  end
end
```

## Use example
```elixir
iex> c("module_one.ex")
[Billing.Reporter]

iex> Billing.Reporter.total()
120

iex> c("module_two.ex")
warning: redefining module Billing.Reporter (current version defined in memory)
module_two.ex:1
[Billing.Reporter]

iex> Billing.Reporter.average()
15

iex> Billing.Reporter.total()
** (UndefinedFunctionError) function Billing.Reporter.total/0 is undefined or private
```
