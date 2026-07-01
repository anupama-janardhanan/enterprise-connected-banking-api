---
title: Pagination (cursor-based)
deprecated: false
hidden: false
metadata:
  robots: index
---
All `GET` listing endpoints support cursor-based pagination.

### Request query parameters

| Param    | Type   | Description                                                                             |
| -------- | ------ | --------------------------------------------------------------------------------------- |
| `cursor` | string | Opaque cursor returned by the previous response. Omit / leave blank for the first page. |
| `limit`  | int    | Page size. Default `25`, maximum `100`.                                                 |

### Response — pagination block

Pagination metadata is returned at the **top-level&#x20;**`meta` object (not inside `data`).

```json
"meta": {
  "timestamp": "2026-04-15T12:00:00Z",
  "pagination": {
    "cursor": null,
    "next_cursor": "eyJpZCI6IjI1In0=",
    "limit": 25,
    "has_more": true
  }
}
```

When `has_more` is `false`, `next_cursor` will be `null`.
