---
title: Error Codes
deprecated: false
hidden: false
metadata:
  robots: index
---
> The authoritative list of error codes (including any future additions) is maintained in the [Enterprise Connected Banking Error Code Details sheet](https://docs.google.com/spreadsheets/d/1J0LeE_WSdRrk6hXvSTtvffo0BmumeXqYA3Dd8_t-dQU/edit?usp=sharing). Use this document as a snapshot; the sheet is the source of truth.

### Account Linking (`OPN_CB_AL_*`)

| Code            | HTTP | Message                                                              | Description                                                                                                                               |
| --------------- | ---- | -------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `OPN_CB_AL_001` | 412  | An Account linking for this particular bank is already under process | A sub-merchant can only initiate a single linking request per online partner bank. The block lasts 1 day, except SBI (multi-day process). |
| `OPN_CB_AL_002` | 400  | Invalid Request                                                      | Request data does not match the API contract. Verify and retry.                                                                           |
| `OPN_CB_AL_003` | 500  | Failed to Initiate Account Linking                                   | Internal service error. Contact support.                                                                                                  |
| `OPN_CB_AL_004` | 412  | Invalid bank\_account\_id                                            | Account not found. Retry with a valid account or contact support.                                                                         |
| `OPN_CB_AL_005` | 412  | Duplicate enterprise\_request\_id provided                           | `enterprise_request_id` already exists; supply a unique one.                                                                              |
| `OPN_CB_AL_006` | 500  | Failed to Fetch Bank Accounts                                        | Internal service error while listing bank accounts.                                                                                       |

### Beneficiary Addition (`OPN_CB_BN_*`)

| Code            | HTTP | Message                                                                    | Description                                                                                                                                                  |
| --------------- | ---- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `OPN_CB_BN_001` | 412  | Duplicate beneficiary details provided                                     | Same beneficiary already exists.                                                                                                                             |
| `OPN_CB_BN_002` | 400  | Invalid Request                                                            | Request data does not match the API contract.                                                                                                                |
| `OPN_CB_BN_003` | 412  | Invalid bank\_account\_id                                                  | Account not found.                                                                                                                                           |
| `OPN_CB_BN_004` | 412  | Invalid open\_beneficiary\_id                                              | Beneficiary not found.                                                                                                                                       |
| `OPN_CB_BN_005` | 500  | Failed to Initiate Bene Creation                                           | Internal service error.                                                                                                                                      |
| `OPN_CB_BN_006` | 412  | Beneficiary creation not available for this bank                           | Beneficiary management is only supported for AXIS Bank RIB accounts. AXIS CIB (NFC) and all other partner banks do not support beneficiary pre-registration. |
| `OPN_CB_BN_007` | 412  | Your account isn't active. Please activate it before adding a beneficiary. | Beneficiaries can only be added once the account is active.                                                                                                  |
| `OPN_CB_BN_008` | 404  | Sub-merchant not found.                                                    | The supplied `sub_merchant_id` does not exist or does not belong to the authenticated merchant.                                                              |

### Fund Transfer (`OPN_CB_FT_*`)

| Code            | HTTP | Message                                                                                   | Description                                                                                                                                                                       |
| --------------- | ---- | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `OPN_CB_FT_001` | 400  | Invalid Request                                                                           | Request data does not match the API contract.                                                                                                                                     |
| `OPN_CB_FT_002` | 412  | Invalid bank\_account\_id                                                                 | Account not found.                                                                                                                                                                |
| `OPN_CB_FT_003` | 500  | Failed to Initiate Fund Transfer                                                          | Internal service error.                                                                                                                                                           |
| `OPN_CB_FT_004` | 412  | Invalid open\_beneficiary\_id                                                             | Beneficiary not found.                                                                                                                                                            |
| `OPN_CB_FT_005` | 412  | Duplicate merchant\_transfer\_batch\_id                                                   | `merchant_transfer_batch_id` already exists; supply a unique one.                                                                                                                 |
| `OPN_CB_FT_006` | 412  | Duplicate merchant\_txn\_ref\_id                                                          | `merchant_txn_ref_id` already exists; supply a unique one.                                                                                                                        |
| `OPN_CB_FT_007` | 412  | Your account isn't active. Please activate it before initiating Fund Transfer.            | Fund transfers can only be initiated once the account is active.                                                                                                                  |
| `OPN_CB_FT_008` | 412  | Amount exceeds the maximum limit for IMPS transfer.                                       | IMPS transactions are limited to 5,00,000 per transaction. Reduce the amount or use RTGS (`transaction_type_id: 3`).                                                              |
| `OPN_CB_FT_009` | 412  | Amount is below the minimum required for RTGS transfer.                                   | RTGS transactions require a minimum amount of 2,00,001. Increase the amount or use NEFT/IMPS.                                                                                     |
| `OPN_CB_FT_010` | 412  | Amount exceeds the maximum limit for NEFT transfer.                                       | NEFT transactions are limited to 2,00,000 per transaction. Reduce the amount or use RTGS (`transaction_type_id: 3`).                                                              |
| `OPN_CB_FT_011` | 412  | `transaction_token` is mandatory for ICICI debit accounts.                                | Fund Transfer Submit must carry the `transaction_token` that ICICI delivered alongside the OTP.                                                                                   |
| `OPN_CB_FT_012` | 412  | `bene_lei` is mandatory for ICICI debit accounts when the batch total is >= INR 50 Crore. | Supply the 20-character Legal Entity Identifier of the beneficiary on Fund Transfer Submit.                                                                                       |
| `OPN_CB_FT_013` | 412  | Bulk fund transfer is not supported for this bank.                                        | Only ICICI (`partner_bank_id` 1), YES Bank (3), and AXIS (12) accept `payout_data` with more than one element. For every other partner bank, submit one payout per Initiate call. |
| `OPN_CB_FT_014` | 404  | Invalid merchant\_transfer\_batch\_id                                                     | The provided `merchant_transfer_batch_id` does not match any fund transfer batch for this account.                                                                                |
| `OPN_CB_FT_015` | 404  | Invalid open\_txn\_id                                                                     | The provided `open_txn_id` does not match any fund transfer transaction for this account.                                                                                         |

> Amount limits per transfer mode are documented in the [Transfer Modes](https://docs.google.com/spreadsheets/d/1JJF7pK9citq04WsrNhU-Z9L1KhWI6OgoIZw3BTfhR3k/edit?usp=sharing) page of the OPEN Connected Banking Glossary.

### Statements (`OPN_CB_ST_*`)

| Code            | HTTP | Message                                                  | Description                                                           |
| --------------- | ---- | -------------------------------------------------------- | --------------------------------------------------------------------- |
| `OPN_CB_ST_001` | 400  | Invalid Request                                          | Request data does not match the API contract.                         |
| `OPN_CB_ST_002` | 412  | Invalid bank\_account\_id                                | Account not found.                                                    |
| `OPN_CB_ST_003` | 500  | Failed to Fetch Statements                               | Internal service error.                                               |
| `OPN_CB_ST_004` | 412  | Invalid date range                                       | `to_date` must be on or after `from_date`.                            |
| `OPN_CB_ST_005` | 412  | Invalid request\_id                                      | `request_id` is unknown (HDFC list endpoint).                         |
| `OPN_CB_ST_006` | 409  | Statement not ready yet                                  | HDFC statement is still being fetched. Wait for the webhook or retry. |
| `OPN_CB_ST_007` | 412  | Statement duration exceeds the maximum allowed (30 days) | The gap between `from_date` and `to_date` must not exceed 30 days.    |

### Balance (`OPN_CB_BL_*`)

| Code            | HTTP | Message                   | Description                                   |
| --------------- | ---- | ------------------------- | --------------------------------------------- |
| `OPN_CB_BL_001` | 400  | Invalid Request           | Request data does not match the API contract. |
| `OPN_CB_BL_002` | 412  | Invalid bank\_account\_id | Account not found.                            |
| `OPN_CB_BL_003` | 500  | Failed to Fetch Balance   | Internal service error.                       |

***

<br />
