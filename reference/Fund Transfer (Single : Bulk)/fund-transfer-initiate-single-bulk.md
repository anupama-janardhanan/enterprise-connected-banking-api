---
title: Fund Transfer Initiate (Single / Bulk
deprecated: false
hidden: false
metadata:
  robots: index
---
`POST /v1/connected_banking/transfer/initiate`

Initiates one or more payouts in a single batch. An OTP is sent and must be confirmed via the `Submit` API (5.2.2).

**Request body — AXIS RIB** (beneficiary pre-registered via Section 5.1)

```json
{
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
  "merchant_transfer_batch_id": "batch_asfg5234",
  "transaction_type_id": 4,
  "payout_data": [
    {
      "merchant_txn_ref_id": "txn_1121",
      "open_beneficiary_id": "275fcb13-d688-481c-a720-1e0789e78f50",
      "vendor_name": "",
      "amount": "45.00",
      "bank_account_number": "",
      "ifsc": "",
      "remarks": "Equity fund"
    },
    {
      "merchant_txn_ref_id": "txn_2213",
      "open_beneficiary_id": "d1087c85-0a2c-409d-860d-c7e4210711f5",
      "vendor_name": "John Doe",
      "amount": "123.98",
      "bank_account_number": "23456543234111",
      "ifsc": "KKBK0123444",
      "remarks": "remark-2"
    }
  ]
}
```

**Request body — AXIS CIB / NFC** (no beneficiary pre-registration required)

```json
{
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "bank_account_id": "c3a1e520-7d9f-4b2e-8f10-2e4c9a6d3f87",
  "merchant_transfer_batch_id": "batch_nfc_001",
  "transaction_type_id": 4,
  "payout_data": [
    {
      "merchant_txn_ref_id": "txn_nfc_001",
      "open_beneficiary_id": "",
      "vendor_name": "Acme Suppliers Pvt Ltd",
      "amount": "75000.00",
      "bank_account_number": "50200012345678",
      "ifsc": "UTIB0000001",
      "remarks": "Invoice INV-2026-0042"
    },
    {
      "merchant_txn_ref_id": "txn_nfc_002",
      "open_beneficiary_id": "",
      "vendor_name": "Global Logistics Ltd",
      "amount": "12500.00",
      "bank_account_number": "00112233445566",
      "ifsc": "HDFC0001234",
      "remarks": "Freight charges May 2026"
    }
  ]
}
```

> For AXIS CIB / NFC accounts, `open_beneficiary_id` must be empty or omitted. Supply `vendor_name`, `bank_account_number`, and `ifsc` for each payout. Bulk submissions (multiple elements in `payout_data`) are supported for AXIS CIB.

| Field                                                        | Required    | Notes                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `merchant_transfer_batch_id`                                 | yes         | Unique per request from the merchant. Must be unique across all batches submitted by the merchant.                                                                                                                                                                                                                                          |
| `transaction_type_id`                                        | yes         | Transaction type identifier. Refer to the [Transaction Type IDs sheet](https://docs.google.com/spreadsheets/d/10BfLvOsDIaXX-9zeGRYCXJzrPlab93xN6-0-PXU4-fQ/edit?usp=sharing) for the full list.                                                                                                                                             |
| `payout_data[]`                                              | yes         | Array of one or more payouts. **Bulk submissions (more than one element) are only accepted when the debit account's&#x20;**`partner_bank_id`**&#x20;is&#x20;**`1`**&#x20;(ICICI),&#x20;**`3`**&#x20;(YES Bank), or&#x20;**`12`**&#x20;(AXIS). For every other partner bank,&#x20;**`payout_data`**&#x20;must contain exactly one element.** |
| `payout_data[].merchant_txn_ref_id`                          | yes         | Unique per payout (within the batch and across the merchant's history).                                                                                                                                                                                                                                                                     |
| `payout_data[].open_beneficiary_id`                          | conditional | **Mandatory when the debit account is an AXIS Bank RIB account.** Obtain it via the Beneficiary Management APIs (Section 5.1). Not required (and not used) for AXIS CIB (NFC) accounts.                                                                                                                                                     |
| `payout_data[].vendor_name` / `bank_account_number` / `ifsc` | conditional | Mandatory when `open_beneficiary_id` is not supplied — i.e. for all non-AXIS partner banks **and for AXIS CIB (NFC) accounts**.                                                                                                                                                                                                             |
| `payout_data[].remarks`                                      | no          | Optional                                                                                                                                                                                                                                                                                                                                    |

**Success —&#x20;**`200 OK`

```json
{
  "status": "success",
  "data": {
    "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
    "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
    "merchant_transfer_batch_id": "batch_asfg5234",
    "payouts": [
      {
        "merchant_txn_ref_id": "txn_1121",
        "open_beneficiary_id": "275fcb13-d688-481c-a720-1e0789e78f50",
        "vendor_name": "",
        "amount": "45.00",
        "bank_account_number": "",
        "ifsc": "",
        "remarks": "Equity fund",
        "open_txn_id": "oxn1324sfc",
        "bank_txn_ref_id": "AXIS1121",
        "status": "request_initiated",
        "bank_status_code": "",
        "error": null
      }
    ]
  },
  "error": null,
  "meta": { "timestamp": "2026-04-15T12:00:00Z" }
}
```

> `bank_status_code` reflects the raw status from the partner bank (e.g. `INITIATED`, `COMPLETED`, `RETURNED_FROM_BENEFICIARY`). Refer to the [Bank Transaction statuses sheet](https://docs.google.com/spreadsheets/d/13uyGwLhsSDbmVCnmc1Qunr_pkxhtPuuJnGuUmJNzKKo/edit?gid=2034142666#gid=2034142666) for the full list.

**Possible errors:** `OPN_CB_FT_001`, `OPN_CB_FT_002`, `OPN_CB_FT_004`, `OPN_CB_FT_005`, `OPN_CB_FT_006`, `OPN_CB_FT_007`, `OPN_CB_FT_008`, `OPN_CB_FT_009`, `OPN_CB_FT_010`, `OPN_CB_FT_013`

> Refer to the [Bank Transaction statuses sheet](https://docs.google.com/spreadsheets/d/13uyGwLhsSDbmVCnmc1Qunr_pkxhtPuuJnGuUmJNzKKo/edit?gid=2034142666#gid=2034142666) for the full list of `status` and `bank_status_code` values returned by each partner bank.

<br />
