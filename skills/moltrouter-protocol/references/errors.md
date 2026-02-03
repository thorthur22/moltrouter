# Error Model

## ERROR Payload
```json
{
  "code": "MRP_AUTH_REQUIRED",
  "message": "Missing or invalid auth token",
  "retryable": false,
  "retry_after_ms": 0,
  "details": {"required_scopes": ["mrp:execute"]}
}
```

## Standard Error Codes
- `MRP_AUTH_REQUIRED`
- `MRP_SCOPE_DENIED`
- `MRP_VERSION_UNSUPPORTED`
- `MRP_RATE_LIMITED`
- `MRP_BUDGET_EXCEEDED`
- `MRP_POLICY_VIOLATION`
- `MRP_PAYMENT_REQUIRED`
- `MRP_PAYMENT_FAILED`
- `MRP_JOB_NOT_FOUND`
- `MRP_INVALID_REQUEST`
- `MRP_INTERNAL_ERROR`
