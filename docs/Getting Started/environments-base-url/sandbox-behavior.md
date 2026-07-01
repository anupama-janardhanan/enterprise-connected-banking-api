---
title: Sandbox Behavior
deprecated: false
hidden: true
metadata:
  robots: index
---
In the **sandbox environment**, no third-party or bank API calls are made. Responses are mocked deterministically based on specific trigger values you supply in the request. See [Section 1.1](#11-sandbox-mock-triggers) for the full list of trigger values per API.

Outside the trigger values listed, each sandbox API returns a default "happy path" response (documented per API).

***

## 1.1 Sandbox Mock Triggers

These trigger values are **only active when the request is authenticated with a sandbox key** (`ACCESS_KEY` prefixed with `sb_`). In the live environment, these values have no special meaning.

### 1.1.1 Beneficiary Initiate — triggered by `ifsc`

| `ifsc`          | Response `beneficiary_status`                  | Notes                                                              |
| --------------- | ---------------------------------------------- | ------------------------------------------------------------------ |
| `TEST0000001`   | `failed`                                       | Mocks an invalid-IFSC failure from the bank.                       |
| `TEST0000002`   | `request_initiated`, then `failed` via webhook | Short-lived failure after initiation.                              |
| `TEST0000003`   | `pending_for_approval`                         | Stays in maker-checker queue.                                      |
| `TEST0000004`   | `active`                                       | Beneficiary becomes active without an OTP step.                    |
| any other value | `request_initiated` (default)                  | Proceed to call Beneficiary Complete with one of the trigger OTPs. |

### 1.1.2 Beneficiary Complete — triggered by `otp`

| `otp`           | Response `beneficiary_status`  |
| --------------- | ------------------------------ |
| `000001`        | `failed`                       |
| `000002`        | `active`                       |
| `000003`        | `pending_for_approval`         |
| any other value | `in_cool_off_period` (default) |

### 1.1.3 Fund Transfer Initiate — triggered by `merchant_txn_ref_id` prefix

Applies to each individual payout row. Use the prefix on the `merchant_txn_ref_id` value, e.g. `FAIL_txn_1121`.

| Prefix                         | Response `status`             |
| ------------------------------ | ----------------------------- |
| `FAIL_`                        | `failed`                      |
| `SUCC_`                        | `success`                     |
| `PEND_`                        | `pending`                     |
| any other (no matching prefix) | `request_initiated` (default) |

### 1.1.4 Fund Transfer Submit — triggered by `otp`

| `otp`     | Per-payout `status` (before batch completeness) |
| --------- | ----------------------------------------------- |
| `000001`  | `failed`                                        |
| `000002`  | `pending`                                       |
| `000003`  | `initiated`                                     |
| `000004`  | `pending_from_bank`                             |
| `000005`  | `otp_initiated`                                 |
| `000006`  | `sent_to_beneficiary`                           |
| `000007`  | `returned`                                      |
| `000008`  | `invalid_otp`                                   |
| `000009`  | `token_expired`                                 |
| any other | `success` (default)                             |

### 1.1.5 Balance — no trigger

Each call returns a random balance (per bank account) deterministically seeded by `bank_account_id` so repeated calls for the same account return the same number.

### 1.1.6 Statements — no trigger

Each call returns a distinct mock statement dataset per `bank_account_id`. The number of transactions and dates are randomized per account.

> In sandbox mode, webhooks are also fired using these mocked statuses. Configure your webhook URL in the dashboard and you will receive the same event stream as production, using the trigger-driven mock data.

<br />
