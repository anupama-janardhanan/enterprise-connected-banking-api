---
title: Common Response Envelope
deprecated: false
hidden: true
metadata:
  robots: index
---
All merchant-facing APIs return responses in a uniform envelope.

### Success

```json
{
  "status": "success",
  "data": { ... },
  "error": null,
  "meta": {
    "timestamp": "2026-04-15T12:00:00Z"
  }
}
```

### Failure

```json
{
  "status": "failed",
  "data": null,
  "error": {
    "code": "OPN_CB_BN_002",
    "message": "Invalid Request.",
    "details": [
      { "field": "ifsc", "issue": "ifsc is invalid" }
    ]
  },
  "meta": {
    "timestamp": "2026-04-15T12:00:00Z"
  }
}
```

`error.details` is an array of field-level issues. It may be empty for non field-specific failures.

> Webhooks (server → merchant) **do not** use this envelope. They are flat JSON payloads.

<br />
