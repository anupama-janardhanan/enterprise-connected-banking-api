---
title: List Beneficiaries
deprecated: false
hidden: true
metadata:
  robots: index
---
`GET /v1/connected_banking/beneficiaries`

Returns paginated beneficiaries for the merchant.

**Query parameters**

| Param                 | Required | Description                                                        |
| --------------------- | -------- | ------------------------------------------------------------------ |
| `sub_merchant_id`     | yes      | Provided by Open                                                   |
| `bank_account_id`     | no       | Filter by merchant's debit account                                 |
| `open_beneficiary_id` | no       | Filter by a specific beneficiary                                   |
| `search`              | no       | Free-text search over `vendor_name`, `bank_account_number`, `ifsc` |
| `cursor`              | no       | Pagination cursor                                                  |
| `limit`               | no       | Page size, default `25`, max `100`                                 |

**Success —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "bene_details": [
      {
        "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
        "open_beneficiary_id": "d6ec35ac-3b18-4060-853e-0416dae2c676",
        "bank_beneficiary_id": "AXIS_BENE_SHGDC6356",
        "vendor_name": "John Doe",
        "bank_account_number": "124514111",
        "ifsc": "KKBK0001122",
        "beneficiary_status": "active"
      }
    ]
  },
  "error": null,
  "meta": {
    "timestamp": "2026-04-15T12:00:00Z",
    "pagination": {
      "cursor": null,
      "next_cursor": "eyJpZCI6IjI1In0=",
      "limit": 25,
      "has_more": true
    }
  }
}
```

**Possible errors:** `OPN_CB_BN_002`