---
title: List All Bank Accounts
deprecated: false
hidden: false
metadata:
  robots: index
---
`GET /v1/connected_banking/bank_accounts`

Returns all linked bank accounts.

**Query parameters**

| Param             | Required | Notes                                                                        |
| ----------------- | -------- | ---------------------------------------------------------------------------- |
| `sub_merchant_id` | no       | Filter to one sub-merchant. Omit to return all accounts across the merchant. |
| `bank_account_id` | no       | Filter to a specific account                                                 |
| `search`          | no       | Free-text search over account number, IFSC, identifier, account name         |
| `partner_bank_id` | no       | Filter accounts of a specific partner bank                                   |
| `cursor`          | no       | Pagination cursor from previous response `next_cursor`. Omit for first page. |
| `limit`           | no       | Page size, default `25`, max `100`                                           |

**Success —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "bank_accounts": [
      {
        "status": "pending",
        "partner_bank_id": 1,
        "bank_account_id": "71c43886-3f4e-57d0-856d-9b038dcd7233",
        "account_number": "**********6745",
        "ifsc": "ICIC0000004",
        "account_name": "Nippon Equity Fund 11",
        "identifier": "equity"
      },
      {
        "status": "active",
        "partner_bank_id": 3,
        "bank_account_id": "gd53533886-3f4e-57d0-856d-9b038dcd7321",
        "account_number": "**********3142",
        "ifsc": "YES001222222",
        "account_name": "Nippon Equity Fund 12",
        "identifier": "equity"
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

**Possible errors:** `OPN_CB_AL_002`, `OPN_CB_AL_004`, `OPN_CB_AL_006`

> The `partner_bank_id` field maps to the partner bank identifier documented in the [Partner bank list](https://docs.google.com/spreadsheets/d/16JhNplmxVA7atE8WKMcALl80Q4InBcGpWc79DmE7QhU/edit?usp=sharing).

<br />
