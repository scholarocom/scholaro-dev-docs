# Events Reference

This page lists the Scholaro webhook events for GPA reports and digital evaluations, the value each one sends in the `type` field, and example payloads.

## Event names and `type` values

The webhook page in Scholaro lists events by a short name. Data events are sent to your endpoint as JSON, and the `type` field in the request body carries the event code shown below, not the short name.

### Data events

| Event in Scholaro | `type` value in the payload |
| --- | --- |
| `gpa_report.created (data)` | `premium-webhook-event-gpa-report-created-data` |
| `gpa_report.updated (data)` | `premium-webhook-event-gpa-report-updated-data` |
| `digital_evaluation.created (data)` | `premium-webhook-event-digital-evaluation-created-data` |
| `digital_evaluation.updated (data)` | `premium-webhook-event-digital-evaluation-updated-data` |

### PDF events

| Event in Scholaro | Event code |
| --- | --- |
| `gpa_report.created (pdf)` | `premium-webhook-event-gpa-report-created-pdf` |
| `gpa_report.updated (pdf)` | `premium-webhook-event-gpa-report-updated-pdf` |
| `digital_evaluation.created (pdf)` | `premium-webhook-event-digital-evaluation-created-pdf` |
| `digital_evaluation.updated (pdf)` | `premium-webhook-event-digital-evaluation-updated-pdf` |

PDF events do not post JSON to your endpoint. When one occurs, Scholaro saves a copy of the report and a nightly process delivers the file. See [PDF Delivery Events](pdf-delivery-events.md).

## Payload structure

Every data event is sent as an HTTP `POST` with a JSON body in this shape:

```json
{
  "object": "event",
  "type": "premium-webhook-event-gpa-report-created-data",
  "data": {
    "...": "..."
  }
}
```

| Field | Description |
| --- | --- |
| `object` | Always `event` |
| `type` | The event code from the data events table above |
| `data` | The GPA report or digital evaluation |

Fields with no value are sent as `null`.

## GPA report payload example

Sent for `gpa_report.created (data)` and `gpa_report.updated (data)`.

```json
{
  "object": "event",
  "type": "premium-webhook-event-gpa-report-created-data",
  "data": {
    "id": 192904,
    "report_name": "Bachelor of Commerce",
    "full_name": "Priya Sharma",
    "date": "2026-04-23T15:11:34.09",
    "dob": "12/1/2002",
    "cust_field1": "1234567",
    "cust_field2": "001234567",
    "cust_field3": "2",
    "country": "India",
    "institution": "Osmania University",
    "qualification": "Bachelor of Commerce",
    "subject": "Finance",
    "grading_scale": "Most Common",
    "gpa": 3.77,
    "total_credits": 120.0,
    "total_points": 452.4,
    "courses": null
  }
}
```

`courses` is `null` unless course details are enabled for your webhook.

## Digital evaluation payload example

Sent for `digital_evaluation.created (data)` and `digital_evaluation.updated (data)`.

```json
{
  "object": "event",
  "type": "premium-webhook-event-digital-evaluation-created-data",
  "data": {
    "id": 113945,
    "evaluation_name": "Equivalency Report",
    "full_name": "Priya Sharma",
    "date": "2026-04-26T09:14:41.25",
    "cust_field1": "1234567",
    "cust_field2": "001234567",
    "cust_field3": "2",
    "country": "India",
    "institution": "Osmania University",
    "accreditation": "University Grants Commission (UGC)",
    "credential": "Bachelor of Commerce",
    "program_length": 3.0,
    "equivalency": "Three years of undergraduate coursework, comparable to a Bachelor's degree",
    "gpa_report": 192904
  }
}
```

Digital evaluations name the report in `evaluation_name`, not `report_name`. `gpa_report` is the `id` of the linked GPA report, or `null` if the evaluation has none.

## Delivery flow summary for PDF events

1. A supported event occurs
2. Scholaro saves a copy of the report
3. A nightly Azure function sends the file to the configured destination
4. The saved file is deleted from storage after processing

## Related docs

- [Webhooks Overview](overview.md)
- [PDF Delivery Events](pdf-delivery-events.md)
- [TargetX / Salesforce File Delivery](targetx-salesforce-file-delivery.md)
- [Slate Integration Setup](../integrations/slate-integration-setup.md)
