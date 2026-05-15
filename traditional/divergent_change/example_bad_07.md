smell_id:divergent_change

# Example 07

```elixir
defmodule Health.PatientService do
  @moduledoc """
  Scheduling, insurance validation, prescription reminders,
  and patient risk scoring all change for different reasons.
  """

  # Reason to change: appointment scheduling rules
  def available_slot?(doctor, starts_at) do
    doctor.active and DateTime.compare(starts_at, DateTime.utc_now()) == :gt
  end

  # Reason to change: insurance validation policy
  def insurance_accepted?(patient, clinic) do
    patient.insurance_provider in clinic.accepted_providers
  end

  # Reason to change: reminder cadence
  def reminder_message(patient, medication) do
    "#{patient.first_name}, remember to take #{medication.name} at #{medication.time}."
  end

  # Reason to change: medical risk rules
  def risk_score(patient) do
    base = if patient.smoker, do: 20, else: 0
    age_factor = if patient.age > 65, do: 30, else: 10
    base + age_factor + length(patient.conditions) * 5
  end
end
```
