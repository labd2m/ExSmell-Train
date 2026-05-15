smell_id:complex_extractions_in_clauses

# Example 01 - Driver eligibility with repeated body extractions

```elixir
defmodule Accounts.User do
  defstruct [:name, :age, :country, :license_type]
end

defmodule Mobility do
  alias Accounts.User

  def drive(%User{name: name, age: age, country: country, license_type: license_type})
      when age >= 18 and country in ["BR", "US"] and license_type in ["B", "FULL"] do
    "#{name} can drive in #{country} with #{license_type}"
  end

  def drive(%User{name: name, age: age, country: country, license_type: license_type})
      when age < 18 and country in ["BR", "US"] do
    "#{name} is too young to drive in #{country} with #{license_type}"
  end

  def drive(%User{name: name, age: age, country: country, license_type: license_type})
      when age >= 18 and license_type in [nil, ""] do
    "#{name} is old enough in #{country}, but has no valid license (#{inspect(license_type)})"
  end
end
```

This is smelly because each clause extracts `name`, `age`, `country`, and `license_type`, but only some values are relevant to the guards while others are only needed later in the function body.
