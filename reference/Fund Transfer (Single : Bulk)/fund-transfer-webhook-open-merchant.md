---
title: Fund Transfer Webhook (Open → Merchant)
deprecated: false
hidden: false
metadata:
  robots: index
---
`POST <Merchant's Webhook URL>`   ·   **Auth:** Basic

Triggered as each payout in a batch reaches a terminal state.

**Headers**

```
Authorization: Basic <base64(ACCESS_KEY:SECRET_KEY)>
X-Webhook-Signature: sfgdfshgs6765346ghdsfhfsgshffg632764377hgefhdh7346778...
```

**Payload (flat)**

```json
{
  "event": "fund_transfer",
  "sub_merchant_id": "b96673d0-9091-45ed-85dc-a1af045a62d2",
  "bank_account_id": "9748b4ae-457d-57d4-b447-98f9fda8f6a8",
  "merchant_transfer_batch_id": "batch_asfg5234",
  "payouts": [
    {
      "merchant_txn_ref_id": "txn_2213",
      "open_beneficiary_id": "d1087c85-0a2c-409d-860d-c7e4210711f5",
      "vendor_name": "John Doe",
      "amount": "123.98",
      "bank_account_number": "23456543234111",
      "ifsc": "KKBK0123444",
      "remarks": "remark-2",
      "open_txn_id": "oxn1324sdf",
      "bank_txn_ref_id": "HDFC2213",
      "status": "success",
      "bank_status_code": "COMPLETED"
    }
  ]
}
```

<br />
