# Example 9 of **Primitive Obsession**

```elixir
defmodule ClinicScheduler do
  def book_appointment(patient_name, patient_document, doctor_name, specialty, date, start_time, end_time, room, consultation_price, status) do
    cond do
      patient_name == "" ->
        {:error, "patient name required"}

      patient_document == "" ->
        {:error, "patient document required"}

      doctor_name == "" ->
        {:error, "doctor name required"}

      specialty == "" ->
        {:error, "specialty required"}

      status not in ["scheduled", "confirmed", "cancelled"] ->
        {:error, "invalid status"}

      start_time >= end_time ->
        {:error, "invalid time range"}

      consultation_price <= 0 ->
        {:error, "invalid consultation price"}

      true ->
        {:ok,
         %{
           patient_name: patient_name,
           patient_document: patient_document,
           doctor_name: doctor_name,
           specialty: specialty,
           date: date,
           start_time: start_time,
           end_time: end_time,
           room: room,
           consultation_price: consultation_price,
           status: status
         }}
    end
  end
end

# Use examples

ClinicScheduler.book_appointment(
  "Mateus Dias",
  "123.456.789-00",
  "Dra. Ana",
  "Cardiologia",
  "2026-05-20",
  "14:00",
  "14:30",
  "Sala 2",
  250.0,
  "scheduled"
)

ClinicScheduler.book_appointment(
  "Mateus Dias",
  "123.456.789-00",
  "Dra. Ana",
  "Cardiologia",
  "2026-05-20",
  "14:30",
  "14:00",
  "Sala 2",
  250.0,
  "scheduled"
)
# {:error, "invalid time range"}
```

## Why this is a smell

This code represents rich domain concepts using only primitive types:

- appointment status is a plain `string`
- date and time are plain `string`s
- consultation price is a raw `float`
- patient and doctor are just loose strings instead of domain entities

The function is working with concepts that should probably be modeled with dedicated structures such as:

- `Patient`
- `Doctor`
- `Appointment`
- `Money`
- `Date` / `Time`-oriented types

Keeping everything as primitives makes the code brittle:
- the parameter list is long and easy to misuse
- invalid values are easy to pass around
- domain rules stay scattered in conditionals
- the code does not clearly express the business meaning of each value
