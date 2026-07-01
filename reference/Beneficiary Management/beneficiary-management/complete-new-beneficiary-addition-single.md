---
title: Complete New Beneficiary Addition (Single)
deprecated: false
hidden: true
metadata:
  robots: index
---
`POST /v1/connected_banking/beneficiary/complete`

Completes the beneficiary addition by confirming the OTP returned to the merchant's registered mobile.

**Request body**

```json
{
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "open_beneficiary_id": "d6ec35ac-3b18-4060-853e-0416dae2c676",
  "otp": "345123"
}
```

**Success —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
    "open_beneficiary_id": "d6ec35ac-3b18-4060-853e-0416dae2c676",
    "bank_beneficiary_id": "AXIS_BENE_SHGDC6356",
    "beneficiary_status": "pending"
  },
  "error": null,
  "meta": { "timestamp": "2026-04-15T12:00:00Z" }
}
```

`beneficiary_status` may be `pending`, `pending_for_approval`, `in_cool_off_period`, `active`, or `failed`.

**Possible errors:** `OPN_CB_BN_002`, `OPN_CB_BN_004`
