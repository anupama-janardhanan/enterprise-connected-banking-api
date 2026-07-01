---
title: List HDFC Statements by request_id
deprecated: false
hidden: false
metadata:
  robots: index
---
`GET /v1/connected_banking/ledger`

For HDFC only. Use after receiving the `statement` webhook (5.3.3) for the corresponding `request_id`.

**Query parameters**

| Param        | Required | Notes                                    |
| ------------ | -------- | ---------------------------------------- |
| `request_id` | yes      | UUID returned by `Fetch Bank Statements` |
| `cursor`     | no       | Pagination cursor                        |
| `limit`      | no       | Page size, default `25`, max `100`       |

**Success —&#x20;**`200 OK`

Same shape as the non-HDFC success in 5.3.1.

**Possible errors:** `OPN_CB_ST_001`, `OPN_CB_ST_005`, `OPN_CB_ST_006`

| Code            | Meaning                                                                                    |
| --------------- | ------------------------------------------------------------------------------------------ |
| `OPN_CB_ST_005` | `request_id` is invalid or unknown                                                         |
| `OPN_CB_ST_006` | The statement is still being fetched from the bank — retry later (or wait for the webhook) |

***

<br />
