---
title: Authentication
deprecated: false
hidden: true
metadata:
  robots: index
---
### Merchant-facing APIs (request → Open)

All merchant-facing endpoints use **Bearer token** authentication. The token is `ACCESS_KEY:SECRET_KEY` (colon-separated).

```
Authorization: Bearer <ACCESS_KEY>:<SECRET_KEY>
```

### Webhooks (Open → merchant)

Webhooks delivered from Open to your configured webhook URL use **HTTP Basic** authentication:

- `username`: `ACCESS_KEY`
- `password`: `SECRET_KEY`

In addition, every webhook request includes an `X-Webhook-Signature` HTTP header containing an HMAC-SHA-256 signature of the raw request body, signed using a **Webhook Signing Secret** that is unique to each webhook URL. This secret is either shared by OPEN during onboarding or can be copied from the **Connected Banking Platform Dashboard** when configuring the webhook URL. See [Section 8](#8-webhook-signature-validation) for the full verification procedure.
