# Webhooks

Use Scholaro webhooks to receive real-time updates when GPA reports and digital evaluations are created or updated.

## Supported event groups

=== "GPA reports"

    - `gpa_report.created (data)`
    - `gpa_report.updated (data)`
    - `gpa_report.created (pdf)`
    - `gpa_report.updated (pdf)`

=== "Digital evaluations"

    - `digital_evaluation.created (data)`
    - `digital_evaluation.updated (data)`
    - `digital_evaluation.created (pdf)`
    - `digital_evaluation.updated (pdf)`

## Setup flow

1. Decide which events you want to receive.
2. Create a public HTTPS endpoint.
3. Accept incoming POST requests.
4. Verify the request signature.
5. Return HTTP `200 OK`.
6. Register your endpoint in the Developer dashboard.

## Event structure

```json
{
  "object": "event",
  "type": "gpa_report.created",
  "data": {
    "...": "..."
  }
}
```

| Field | Description |
| --- | --- |
| `object` | Always `event` |
| `type` | Event name |
| `data` | Payload |

## GPA report payload example

```json
{
  "id": 192904,
  "report_name": "Bachelor of Architecture",
  "full_name": "Aleks M",
  "date": "2022-04-23T03:11:34.09",
  "dob": "12/1/1980",
  "cust_field1": "1234567",
  "cust_field2": "001234567",
  "cust_field3": "2",
  "country": "India",
  "institution": "Osmania University",
  "qualification": "Bachelor of Commerce",
  "subject": "Finance",
  "grading_scale": "Most Common",
  "gpa": 3.769,
  "total_credits": 13.0,
  "total_points": 15.0
}
```

## Digital evaluation payload example

```json
{
  "id": 113945,
  "report_name": "Equivalency Report",
  "full_name": "Alex M",
  "date": "2022-09-26T00:14:41.25",
  "cust_field1": "1234567",
  "cust_field2": "001234567",
  "cust_field3": "2",
  "country": "India",
  "credential": "Bachelor of Architecture",
  "program_length": 5.0,
  "equivalency": "Bachelor's Degree",
  "gpa_report": 192904
}
```

## Signature verification

To verify webhook authenticity:

1. Read the raw request body.
2. Extract the `X-Scholaro-Signature` header.
3. Generate an HMAC SHA256 hash using your signing secret.
4. Encode the result as Base64.
5. Compare it to the header value.

### Pseudocode

```text
signature = Base64(HMAC_SHA256(raw_body, signing_secret))
compare(signature, X-Scholaro-Signature)
```

## Best practices

!!! tip
    Return HTTP `200 OK` as quickly as possible.

!!! tip
    Process webhook jobs asynchronously when possible.

!!! tip
    Log each incoming event and make handlers idempotent.

!!! tip
    Safely handle retries and duplicate deliveries.

## Developer dashboard

```text
https://www.scholaro.com/app/developer/webhooks
```
