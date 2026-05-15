smell_id:shotgun_surgery

# Shotgun Surgery - Example 07

```elixir
defmodule AuditEvents do
  def event_name(:invoice_paid), do: "invoice.paid"
  def event_name(:invoice_failed), do: "invoice.failed"
  # def event_name(:invoice_refunded), do: "invoice.refunded"
end

defmodule MetricsCounters do
  def counter(:invoice_paid), do: "billing_invoice_paid_total"
  def counter(:invoice_failed), do: "billing_invoice_failed_total"
  # def counter(:invoice_refunded), do: "billing_invoice_refunded_total"
end

defmodule WebhookTopics do
  def topic(:invoice_paid), do: "billing.paid"
  def topic(:invoice_failed), do: "billing.failed"
  # def topic(:invoice_refunded), do: "billing.refunded"
end

defmodule BillingEvents do
  def publish(kind, payload) do
    %{
      audit: AuditEvents.event_name(kind),
      metric: MetricsCounters.counter(kind),
      topic: WebhookTopics.topic(kind),
      payload: payload
    }
  end
end
```
