smell_id:large_class

# Example 09

```elixir
defmodule Clinic.PatientDesk do
  alias Clinic.Billing
  alias Clinic.Mailer
  alias Clinic.PDF

  # Registration
  def register_patient(attrs), do: {:ok, attrs}
  def update_address(patient, address), do: %{patient | address: address}

  # Appointment scheduling
  def schedule_appointment(patient, doctor, date) do
    %{patient_id: patient.id, doctor_id: doctor.id, date: date}
  end

  def cancel_appointment(appointment) do
    %{appointment | status: :cancelled}
  end

  # Prescriptions
  def create_prescription(doctor, patient, medicine) do
    %{doctor_id: doctor.id, patient_id: patient.id, medicine: medicine}
  end

  # Billing
  def charge_consultation(patient, amount) do
    Billing.charge(patient.account_id, amount)
  end

  # Notifications
  def send_reminder(patient, appointment) do
    Mailer.send(patient.email, "Reminder: #{appointment.date}")
  end

  # Exports / reports
  def export_medical_summary(patient) do
    PDF.render("summary-#{patient.id}.pdf", patient)
  end
end
```
