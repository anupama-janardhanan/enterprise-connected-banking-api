---
title: HDFC Statement Ready Webhook (Open → Merchant)
deprecated: false
hidden: true
metadata:
  robots: index
---
`POST <Merchant's Webhook URL>`   ·   **Auth:** Basic

Triggered when an HDFC statement requested via 5.3.1 is ready. The webhook is **notify-only** — it does not include the statement data. Call 5.3.2 with the `request_id` to retrieve it.

**Headers**

```
Authorization: Basic <base64(ACCESS_KEY:SECRET_KEY)>
X-Webhook-Signature: b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90ff...
```

**Payload (flat)**

```json
{
  "event": "statement",
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "bank_account_id": "a7973e36-ad33-5de9-86d0-c448d8f47e87",
  "partner_bank_id": 8,
  "request_id": "f4e8b2a1-9c0d-4a1e-9b2c-7d6e5f4a3b21",
  "status": "ready"
}
```

<br />
