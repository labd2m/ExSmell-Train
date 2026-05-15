smell_id:large_class

# Example 05

```elixir
defmodule School do
  alias Campus.Billing
  alias Campus.Messenger

  # Enrollment
  def enroll(student, course) do
    {:ok, %{student_id: student.id, course_id: course.id}}
  end

  def drop(student, course) do
    {:ok, %{student_id: student.id, course_id: course.id}}
  end

  # Attendance
  def mark_present(student, class_id) do
    {:ok, {student.id, class_id, :present}}
  end

  def mark_absent(student, class_id) do
    {:ok, {student.id, class_id, :absent}}
  end

  # Grading
  def record_grade(student, course, score) do
    %{student_id: student.id, course_id: course.id, score: score}
  end

  def calculate_average(scores) do
    Enum.sum(scores) / length(scores)
  end

  # Billing
  def charge_tuition(student, amount) do
    Billing.charge(student.account_id, amount)
  end

  def apply_scholarship(amount, :full), do: 0
  def apply_scholarship(amount, :partial), do: amount * 0.5
  def apply_scholarship(amount, _), do: amount

  # Notifications
  def send_absence_alert(parent, class_name) do
    Messenger.sms(parent.phone, "Absence recorded for #{class_name}")
  end

  # Transcript export
  def export_transcript(student) do
    "Transcript for #{student.name}"
  end
end
```
