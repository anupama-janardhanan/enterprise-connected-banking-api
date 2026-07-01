---
title: Environments & Base URL
deprecated: false
hidden: true
metadata:
  robots: index
---
**Base URL (sandbox & live):** `https://enterprise-api.open.money`

| Environment | Base URL                            | Credentials                         |
| ----------- | ----------------------------------- | ----------------------------------- |
| Sandbox     | `https://enterprise-api.open.money` | Sandbox `ACCESS_KEY` / `SECRET_KEY` |
| Live        | `https://enterprise-api.open.money` | Live `ACCESS_KEY` / `SECRET_KEY`    |

> The base URL is identical for sandbox and live. Only the API key pair differs. All endpoint paths in this guide are relative to this base URL (e.g. `POST /v1/connected_banking/beneficiary/initiate` resolves to `https://enterprise-api.open.money/v1/connected_banking/beneficiary/initiate`).

### Where to get the keys

Login to your Connected Banking Dashboard, then navigate to:

`Settings → Developer API`

You can generate and rotate **separate key pairs for sandbox and live** from this section.
