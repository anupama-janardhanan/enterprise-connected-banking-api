---
title: Sandbox Behavior
deprecated: false
hidden: true
metadata:
  robots: index
---
In the **sandbox environment**, no third-party or bank API calls are made. Responses are mocked deterministically based on specific trigger values you supply in the request. See [Section 1.1](#11-sandbox-mock-triggers) for the full list of trigger values per API.

Outside the trigger values listed, each sandbox API returns a default "happy path" response (documented per API).
