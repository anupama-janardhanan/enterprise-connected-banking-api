---
title: Get Fund Transfer Status
deprecated: false
hidden: false
metadata:
  robots: index
---
`GET /v1/connected_banking/transfer`

Polls the current status of one or more payouts.

**Query parameters**

| Param                        | Required    | Notes                                                             |
| ---------------------------- | ----------- | ----------------------------------------------------------------- |
| `merchant_transfer_batch_id` | conditional | Either `merchant_transfer_batch_id` or `open_txn_id` is mandatory |
| `open_txn_id`                | conditional | Either `merchant_transfer_batch_id` or `open_txn_id` is mandatory |
| `merchant_txn_ref_id`        | no          | Filter to a specific payout in the batch                          |
| `cursor`                     | no          | Pagination cursor                                                 |
| `limit`                      | no          | Page size, default `25`, max `100`                                |

**Success —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
    "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
    "merchant_transfer_batch_id": "batch_asfg5234",
    "payouts": [ /* same shape as 5.2.1, includes bank_status_code */ ]
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

**Possible errors:** `OPN_CB_FT_001`, `OPN_CB_FT_014`, `OPN_CB_FT_015`

***

### 5.3 Banking Statements

#### 5.3.1 Fetch Bank Statements

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
