---
title: Initiate New Beneficiary Addition (Single)
deprecated: false
hidden: true
metadata:
  robots: index
---
`POST /v1/connected_banking/beneficiary/initiate`

Initiates beneficiary addition. An OTP is sent to the registered mobile number. Use the `Complete` API (5.1.2) to confirm with the OTP.

**Request body**

```json
{
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
  "vendor_name": "John Doe",
  "bank_account_number": "124514111",
  "ifsc": "KKBK0001122"
}
```

| Field                 | Type   | Required | Description                                  |
| --------------------- | ------ | -------- | -------------------------------------------- |
| `sub_merchant_id`     | string | yes      | Provided by Open                             |
| `bank_account_id`     | string | yes      | Open's UUID for the merchant's debit account |
| `vendor_name`         | string | yes      | Beneficiary name                             |
| `bank_account_number` | string | yes      | Beneficiary account number                   |
| `ifsc`                | string | yes      | Beneficiary IFSC                             |

**Success —&#x20;**`201 Created`

```json
{
  "status": "success",
  "data": {
    "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
    "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
    "open_beneficiary_id": "d6ec35ac-3b18-4060-853e-0416dae2c676",
    "bank_beneficiary_id": "",
    "beneficiary_status": "request_initiated"
  },
  "error": null,
  "meta": { "timestamp": "2026-04-15T12:00:00Z" }
}
```

**Possible errors:** `OPN_CB_BN_001`, `OPN_CB_BN_002`, `OPN_CB_BN_003`, `OPN_CB_BN_005`, `OPN_CB_BN_006`, `OPN_CB_BN_007`