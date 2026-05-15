smell_id:divergent_change

# Example 05

```elixir
defmodule School.StudentPortal do
  @moduledoc """
  This module changes for enrollment policies, tuition rules,
  attendance calculations, and parent notifications.
  """

  # Reason to change: enrollment requirements
  def enroll(student, course) do
    if course.seats > 0 and student.gpa >= course.minimum_gpa do
      {:ok, %{student_id: student.id, course_id: course.id}}
    else
      {:error, :cannot_enroll}
    end
  end

  # Reason to change: tuition policy
  def calculate_tuition(student, credits) do
    rate = if student.residency == :local, do: 120, else: 350
    credits * rate
  end

  # Reason to change: attendance rules
  def attendance_percentage(total_classes, attended) do
    attended / total_classes * 100
  end

  # Reason to change: notification templates
  def notify_parent(student, message) do
    Mailer.deliver(%{to: student.parent_email, subject: "Student update", body: message})
  end
end
```
