# Webhooks Overview

Scholaro webhooks can be used to support downstream delivery workflows for report files and related integration events.

Depending on your setup, supported events can trigger saved report copies that are later processed and sent to connected destination systems.

## How the delivery flow works

1. A supported event occurs in Scholaro
2. Scholaro saves a copy of the report for delivery processing
3. A nightly Azure function sends the file to the configured destination
4. The saved file is removed from storage after processing

## What this section covers

- Event references for currently documented webhook events
- PDF delivery behavior for GPA reports and digital evaluations
- Destination-specific setup guidance for systems such as Salesforce
- Related integration setup for downstream systems such as Slate

## Related docs

- [Webhooks Landing Page](../webhooks.md)
- [Events Reference](events-reference.md)
- [PDF Delivery Events](pdf-delivery-events.md)
- [TargetX / Salesforce File Delivery](targetx-salesforce-file-delivery.md)
- [Slate Integration Setup](../integrations/slate-integration-setup.md)