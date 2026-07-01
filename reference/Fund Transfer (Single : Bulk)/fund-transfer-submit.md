---
title: Fund Transfer Submit
deprecated: false
hidden: false
metadata:
  robots: index
---
`POST /v1/connected_banking/transfer/submit`

Confirms the OTP for a previously initiated batch. The bank now processes the payouts; final status is delivered via webhook (5.2.3) and via the `Get Status` API (5.2.4).

**Request body**

```json
{
  "merchant_transfer_batch_id": "batch_asfg5234",
  "otp": "196234",
  "transaction_token": "gsd2613562sdhfsdjh32464678hsdgf723648",
  "bene_lei": "3358005XE3KTPJZP6X27"
}
```

| Field                        | Required    | Notes                                                                                                                                                                                                                                                                                                                                                                         |
| ---------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_transfer_batch_id` | yes         | The batch identifier returned by `Initiate` (5.2.1).                                                                                                                                                                                                                                                                                                                          |
| `otp`                        | yes         | OTP delivered to the registered mobile for the initiated batch.                                                                                                                                                                                                                                                                                                               |
| `transaction_token`          | conditional | **Mandatory only when the debit account is an ICICI Bank account.** Not required for other partner banks. The token is delivered by the bank alongside the OTP and must be echoed back on Submit.                                                                                                                                                                             |
| `bene_lei`                   | conditional | **Mandatory only when the debit account is an ICICI Bank account AND the total transfer amount of the batch is greater than or equal to INR 50 Crore (500,000,000).** Not required below that threshold or for any non-ICICI debit account. A 20-character alphanumeric Legal Entity Identifier (LEI) of the beneficiary, per RBI requirements for large-value RTGS payments. |

**Success —&#x20;**`200 OK`

Same shape as 5.2.1 success, with each payout's `status` updated to `initiated` / `success` / `failed` and `bank_status_code` populated with the partner bank's raw status (e.g. `INITIATED`, `COMPLETED`, `RETURNED_FROM_BENEFICIARY`).

**Possible errors:** `OPN_CB_FT_001`, `OPN_CB_FT_011` (ICICI only -- missing `transaction_token`), `OPN_CB_FT_012` (ICICI only -- missing `bene_lei` when batch total >= INR 50 Crore)

***

<br />
