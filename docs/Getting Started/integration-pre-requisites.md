---
title: Integration Pre-requisites
deprecated: false
hidden: false
metadata:
  robots: index
---
Before starting integration, the following must be in place:

**From OPEN (provided to the merchant):**

- UAT / Sandbox `ACCESS_KEY` and `SECRET_KEY`
- `sub_merchant_id` assigned to the merchant
- **Webhook Signing Secret** -- a unique secret per webhook URL, used to verify the authenticity of webhook payloads (see [Section 8](#8-webhook-hash-validation)). This secret is either shared by OPEN during onboarding or can be copied from the Connected Banking Platform Dashboard when configuring the webhook URL.

**From the Merchant (provided to OPEN):**

- List of **IP addresses to be whitelisted** for API access
- **Webhook URL** to be configured for receiving event notifications (beneficiary addition, fund transfer, statement ready)

> Integration cannot proceed until both sides have exchanged the above. Coordinate with your OPEN account manager to complete the setup.

<br />
