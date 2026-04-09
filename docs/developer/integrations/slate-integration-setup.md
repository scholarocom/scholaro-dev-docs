# Slate Integration Setup

This guide explains how to configure Slate to receive files delivered from Scholaro.

## Overview

The Slate integration uses a source format and a standard POST URL in Slate, together with the Scholaro webhook configuration, to receive ZIP files and map them into the correct Slate records.

## Prerequisites

Before starting, make sure you have:

- Access to Slate as an administrator
- Permission to create users, source formats, and web service endpoints
- Access to the Scholaro developer webhooks page

## Step 1: Create a Slate service account

Create a dedicated Slate user account for the integration.

Use a service account instead of a personal user account so the integration remains stable over time.

## Step 2: Create the source format in Slate

In Slate, create a new source format for the Scholaro integration.

This source format will be used to receive incoming data and files from Scholaro.

## Step 3: Enter the XML layout

Within the source format, add the XML layout required for the integration.

Use the XML layout from your current internal setup or existing help-center documentation.

## Step 4: Copy the Standard POST URL

In the Slate source format, go to the Web Services area and copy the Standard POST URL.

You will use this URL as the hosted endpoint in Scholaro.

## Step 5: Configure Scholaro webhooks

In Scholaro, open the developer webhooks page and configure the webhook destination using the Slate Standard POST URL.

Set the hosted endpoint and the required authentication values based on your Slate setup.

Also select the events that should be used for delivery.

## Step 6: Receive ZIP files from Scholaro

Once configured, Scholaro sends ZIP files to Slate through the configured destination.

These files can then be processed through the Slate source format workflow.

## Step 7: Remap the source format

In Slate, open the source format and remap the incoming data after the first sample files are received.

This ensures Slate recognizes the incoming fields and file structure correctly.

## Step 8: Configure field mappings

Review and map the incoming fields in Slate.

At minimum, confirm the mappings for:

- First Name
- Last Name
- Email
- Material Code
- Filename

## Step 9: Map document types to material types

Map incoming Scholaro document types to the correct Slate material types.

This step is important so documents are stored and classified correctly in Slate.

## Step 10: Reactivate remap settings if needed

After mapping is complete, make sure the source format is placed back into the correct active state for production processing.

## Validation

After setup is complete, send a test file and confirm:

- the file is received by Slate
- the mapped fields populate correctly
- the file is attached to the correct record
- the material type mapping works as expected

## Related docs

- [Webhooks Overview](../webhooks/overview.md)
- [Events Reference](../webhooks/events-reference.md)
- [PDF Delivery Events](../webhooks/pdf-delivery-events.md)