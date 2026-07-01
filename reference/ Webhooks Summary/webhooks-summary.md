---
title: ' Webhooks Summary'
deprecated: false
hidden: true
metadata:
  robots: index
---
| Event                  | Endpoint               | Auth  | Triggered When                                       |
| ---------------------- | ---------------------- | ----- | ---------------------------------------------------- |
| `beneficiary_addition` | Merchant's webhook URL | Basic | Beneficiary status changes                           |
| `fund_transfer`        | Merchant's webhook URL | Basic | Each payout in a batch reaches a terminal state      |
| `statement`            | Merchant's webhook URL | Basic | HDFC statement requested via 5.3.1 is ready to fetch |

All webhooks include an `X-Webhook-Signature` header. **Always validate the signature before acting on the payload.**

***

<br />
