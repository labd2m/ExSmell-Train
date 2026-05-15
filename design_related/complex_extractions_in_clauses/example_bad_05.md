smell_id:complex_extractions_in_clauses

# Example 05 - Report access check with multiple arguments and repeated extraction

```elixir
defmodule Accounts.User do
  defstruct [:id, :role, :department, :active, :name]
end

defmodule Reports.Report do
  defstruct [:id, :department, :sensitivity, :owner_id, :title]
end

defmodule Reports.Policy do
  alias Accounts.User
  alias Reports.Report

  def can_open?(
        %User{id: user_id, role: role, department: user_department, active: active, name: name},
        %Report{
          id: report_id,
          department: report_department,
          sensitivity: sensitivity,
          owner_id: owner_id,
          title: title
        }
      )
      when active == true and role == :admin do
    {:ok, "#{name} can open report #{report_id} (#{title}) with sensitivity #{sensitivity}"}
  end

  def can_open?(
        %User{id: user_id, role: role, department: user_department, active: active, name: name},
        %Report{
          id: report_id,
          department: report_department,
          sensitivity: sensitivity,
          owner_id: owner_id,
          title: title
        }
      )
      when active == true and user_id == owner_id do
    {:ok, "#{name} owns report #{report_id} (#{title}) in #{report_department}"}
  end

  def can_open?(
        %User{id: user_id, role: role, department: user_department, active: active, name: name},
        %Report{
          id: report_id,
          department: report_department,
          sensitivity: sensitivity,
          owner_id: owner_id,
          title: title
        }
      )
      when active == true and user_department == report_department and sensitivity in [:low, :medium] do
    {:ok, "#{name} from #{user_department} can open #{title}; user_role=#{role}; owner=#{owner_id}; report=#{report_id}"}
  end
end
```

This is hard to scan because both arguments extract several fields, but only a subset is used in guards. The rest is noise until the function body.
