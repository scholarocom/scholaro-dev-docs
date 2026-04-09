# Events Reference

This page lists the currently documented Scholaro webhook events related to report PDF delivery.

## Currently documented events

### GPA report events

- `gpa_report.created`
- `gpa_report.updated`

### Digital evaluation events

- `digital_evaluation.created`
- `digital_evaluation.updated`

## Event usage notes

These events can be used for PDF delivery workflows when that feature is enabled.

When one of the supported events occurs, Scholaro saves a copy of the generated report for downstream delivery processing.

## Delivery flow summary

1. A supported event occurs
2. Scholaro saves a copy of the report
3. A nightly Azure function sends the file to the configured destination
4. The saved file is deleted from storage after processing

## Related docs

- [Webhooks Overview](overview.md)
- [PDF Delivery Events](pdf-delivery-events.md)
- [TargetX / Salesforce File Delivery](targetx-salesforce-file-delivery.md)
- [Slate Integration Setup](../integrations/slate-integration-setup.md)