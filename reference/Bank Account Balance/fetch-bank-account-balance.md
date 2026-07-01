---
title: Fetch Bank Account Balance
deprecated: false
hidden: false
metadata:
  robots: index
---
`GET /v1/connected_banking/balance`

Returns latest available balance for one or more accounts.

**Query parameters**

| Param             | Required | Notes                              |
| ----------------- | -------- | ---------------------------------- |
| `sub_merchant_id` | no       | Filter to one sub-merchant         |
| `bank_account_id` | no       | Filter to a specific account       |
| `cursor`          | no       | Pagination cursor                  |
| `limit`           | no       | Page size, default `25`, max `100` |

**Success —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "balances": [
      { "bank_account_id": "71c43886-3f4e-57d0-856d-9b038dcd7233", "balance": "128345712.00" },
      { "bank_account_id": "gfd5354-3f4e-57d0-856d-9b038dcd7233",  "balance": "812363.00"   }
    ]
  },
  "error": null,
  "meta": {
    "timestamp": "2026-04-15T12:00:00Z",
    "pagination": {
      "cursor": null,
      "next_cursor": null,
      "limit": 25,
      "has_more": false
    }
  }
}
```

**Possible errors:** `OPN_CB_BL_001`, `OPN_CB_BL_002`, `OPN_CB_BL_003`

<br />
