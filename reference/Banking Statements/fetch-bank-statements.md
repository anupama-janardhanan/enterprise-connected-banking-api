---
title: Fetch Bank Statements
deprecated: false
hidden: false
metadata:
  robots: index
---
`GET /v1/connected_banking/statements`

Returns the bank statement for a given account and date range.

> See the [Partner bank list](https://docs.google.com/spreadsheets/d/16JhNplmxVA7atE8WKMcALl80Q4InBcGpWc79DmE7QhU/edit?usp=sharing) for the full `partner_bank_id` → bank mapping.
>
> **HDFC special case (**`partner_bank_id = 8`**):** statements are fetched **asynchronously** from the bank. The initial call returns only a `request_id` and HTTP `202 Accepted`. Once the statement is ready, Open notifies your webhook (5.3.3); you then call **List HDFC Statements by request\_id** (5.3.2) to fetch the data.
>
> **All other banks:** statements are returned **inline** in this single call.

**Query parameters**

| Param             | Required | Notes                                                               |
| ----------------- | -------- | ------------------------------------------------------------------- |
| `from_date`       | yes      | `YYYY-MM-DD`                                                        |
| `to_date`         | yes      | `YYYY-MM-DD`. **Maximum window is 30 days.**                        |
| `bank_account_id` | yes      | Open's UUID for the merchant's account                              |
| `cursor`          | no       | Pagination cursor (ignored for HDFC initial fetch)                  |
| `limit`           | no       | Page size, default `25`, max `100` (ignored for HDFC initial fetch) |

**Success (non-HDFC) —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "bank_account_id": "a7973e36-ad33-5de9-86d0-c448d8f47e87",
    "request_id": "f4e8b2a1-9c0d-4a1e-9b2c-7d6e5f4a3b21",
    "statements": [
      {
        "transaction_id": "UPI/CR/32225095826799663304409",
        "transaction_amount": 50.00,
        "transaction_type": "CR",
        "closing_balance": null,
        "transacted_date": "2023-08-10 00:00:00",
        "posted_date": "2023-08-10 00:00:00",
        "value_date": "2023-08-10 00:00:00",
        "reference_number": "UPI/CR/32225095826799663304409",
        "remarks": "001045:DEPTFR:TRANSFERFROM4897735162098:UPI/CR/322250958435/RADHIKA/SBIN/halsoderad/UPI"
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

**Success (HDFC,&#x20;**`partner_bank_id = 8`**) —&#x20;**`202 Accepted`

```json
{
  "status": "success",
  "data": {
    "bank_account_id": "a7973e36-ad33-5de9-86d0-c448d8f47e87",
    "request_id": "f4e8b2a1-9c0d-4a1e-9b2c-7d6e5f4a3b21",
    "statements": null
  },
  "error": null,
  "meta": { "timestamp": "2026-04-15T12:00:00Z" }
}
```

`request_id` is always returned for every bank — store it for reference / reconciliation.

**Possible errors:** `OPN_CB_ST_001`, `OPN_CB_ST_002`, `OPN_CB_ST_003`, `OPN_CB_ST_004`, `OPN_CB_ST_007`

***

<br />
