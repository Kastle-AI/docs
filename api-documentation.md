# Kastle AI - Welcome Call Agent API Documentation

## Overview

This documentation outlines the API integration for Kastle AI's welcome call agent. The system processes leads and makes automated outbound calls to borrowers based on the data you provide.

## Entity Management APIs

All endpoints use the base URL: `https://kapi.prod1.kastle.ai/api/v1/campaigns/entities`

### Create Entity

**Endpoint:** `/create/figure-lending/{campaign_id}`  
**Method:** `POST`  
**Content-Type:** `application/json`

> Note: The `campaign_id` will be provided by your Kastle AI representative.

#### Request Body

```json
{
  "autopay_status": "string",
  "expected_dob": "YYYY-MM-DD",
  "expected_first_name": "string",
  "expected_full_name": "string", 
  "expected_last_name": "string",
  "expected_property_address": "string",
  "payment_date": "YYYY-MM-DD",
  "payment_due": "string",
  "payment_due_date": "YYYY-MM-DD",
  "phone_number": "string",
  "originating_org": "string",
  "loan_number_servicer": "string"
}
```

#### Response

```json
{
  "entity_id": "string",
  "status": "created"
}
```

### Other Entity Endpoints

The following additional endpoints are available for entity management:

- **Get Entity**: `GET /get/figure-lending/{campaign_id}/{entity_id}`
- **Update Entity**: `PUT /update/figure-lending/{campaign_id}/{entity_id}`
- **Delete Entity**: `DELETE /delete/figure-lending/{campaign_id}/{entity_id}`
- **List Entities**: `GET /list/figure-lending/{campaign_id}`

## Callback Configuration

You need to provide a callback endpoint to receive call results from Kastle AI.

#### Request Body (Sent by Kastle AI)

```json
{
  "Note_Type": "Call",
  "Call_Type": "Outgoing",
  "Reason": "Other",
  "Loan": "Loan UUID",
  "Disposition": "Voicemail",
  "Note": "Call summary text with conversation details",
  "Timestamp": "YYYY-MM-DD HH:MM:SS",
  "Duration": "MM:SS"
}
```

#### Data Dump Option

Alternatively, Kastle AI can provide a data dump of all call records if needed. This can be delivered via secure file transfer or API endpoint. Please specify your preferred delivery method when setting up the integration.

## Best Practices

1. Ensure all required fields are provided when creating entities
2. Implement proper error handling for your callback endpoint
3. Provide phone numbers in a standard format
4. Test your integration before going live

## Support

For questions or issues with API integration, contact your Kastle AI representative.
