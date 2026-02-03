# Rate Limits & Pagination

## Rate Limit Headers
Services SHOULD include:
- `MRP-RateLimit-Limit`
- `MRP-RateLimit-Remaining`
- `MRP-RateLimit-Reset`

## Rate Limit Error
Return `ERROR` with `code = "MRP_RATE_LIMITED"` and `retry_after_ms`.

## Pagination
For list responses (registry queries or offers), include:
```json
{
  "page": {"cursor": "next-cursor", "has_more": true}
}
```
