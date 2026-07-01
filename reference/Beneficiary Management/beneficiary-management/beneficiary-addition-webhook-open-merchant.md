---
title: Beneficiary Addition Webhook (Open → Merchant)
deprecated: false
hidden: false
metadata:
  robots: index
---
`POST <Merchant's Webhook URL>`   ·   **Auth:** Basic

Triggered when the beneficiary's status changes (e.g. moves from `pending` to `active` after the cool-off period).

**Headers**

```
Authorization: Basic <base64(ACCESS_KEY:SECRET_KEY)>
X-Webhook-Signature: a1b2c3d4e5f60718293a4b5c6d7e8f90...
```

**Payload (flat -- no envelope)**

```json
{
  "event": "beneficiary_addition",
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
  "open_beneficiary_id": "d6ec35ac-3b18-4060-853e-0416dae2c676",
  "bank_beneficiary_id": "AXIS_BENE_SHGDC6356",
  "beneficiary_status": "active",
  "status_description": "Beneficiary added successfully. You can start transacting after 30 minutes."
}
```

> Validate `X-Webhook-Signature` per [Section 8](#8-webhook-signature-validation) before acting on the payload.

<br />
