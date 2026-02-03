# Payments & Usage

## Payment Intent
Use `payment_intent` during `NEGOTIATE` to pre-authorize spend.
```json
{
  "payment_intent": {
    "intent_id": "pi_123",
    "currency": "usd",
    "max_amount": 0.05,
    "pricing_model": "per_request",
    "payer": "agent:moltbots/alpha",
    "payee": "service:clawdbots/summarize",
    "expires_at": "2025-01-01T00:10:00Z"
  }
}
```

## Usage Report
Return usage in `EVIDENCE`.
```json
{
  "usage": {
    "units": 1,
    "unit_type": "request",
    "amount": 0.02,
    "currency": "usd",
    "meter_id": "meter-abc"
  },
  "settlement": {
    "intent_id": "pi_123",
    "status": "captured",
    "captured_at": "2025-01-01T00:01:00Z"
  }
}
```

## Failure & Refunds
- If execution fails before usage, return `settlement.status = "voided"`.
- If partial usage, return `settlement.status = "partial"` and include `amount`.
