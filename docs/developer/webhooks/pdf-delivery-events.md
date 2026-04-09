# PDF Delivery Events

Scholaro supports PDF delivery workflows for specific GPA report and digital evaluation events.

## Supported events

The following events are supported for PDF delivery:

- `gpa_report.created`
- `gpa_report.updated`
- `digital_evaluation.created`
- `digital_evaluation.updated`

## How it works

If PDF delivery is enabled, Scholaro saves a copy of the report whenever one of the supported events occurs.

A nightly Azure function then sends the saved report to the destination system and deletes the report from the storage container after processing.

## Slate delivery behavior

For Slate integrations, the existing integration flow remains the same. Enabling PDF delivery adds more files to the process rather than replacing the existing setup.

## Related docs

- [Webhooks Overview](overview.md)
- [Events Reference](events-reference.md)
- [Slate Integration Setup](../integrations/slate-integration-setup.md)