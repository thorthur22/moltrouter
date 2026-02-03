# Error Model

## ERROR Payload
```json
{
  "code": "MRP_AUTH_REQUIRED",
  "message": "Missing or invalid auth token",
  "retryable": false,
  "retry_after_ms": 0,
  "details": {"required_scopes": ["mrp:execute"]},
  "caused_by": "00000000-0000-0000-0000-000000000002"
}
```

## Semantics
- `code` MUST be stable, machine-readable, and suitable for programmatic handling.
- `message` is intended for humans and MAY change over time.
- `retryable` indicates whether a retry could succeed without changing inputs.
- `details` is optional structured context for debugging or UI.
- `caused_by` is an optional message id that triggered the error.

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
