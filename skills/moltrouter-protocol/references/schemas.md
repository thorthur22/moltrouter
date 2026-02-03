# MRP Schemas

## Capability Manifest (`application/mrp-manifest+json`)
```json
{
  "capability": "summarize",
  "version": "1.0",
  "inputs": ["text", "url"],
  "outputs": ["markdown", "json"],
  "constraints": {
    "max_input_tokens": 8000,
    "policy": ["no_pii", "consent_required"]
  },
  "cost": {"unit": "usd", "estimate": 0.01},
  "latency": {"p50": "200ms"},
  "proofs_required": ["attestation", "rate_limit"],
  "endpoints": {
    "discover": "/mrp/discover",
    "negotiate": "/mrp/negotiate",
    "execute": "/mrp/execute"
  }
}
```

## DISCOVER Payload
```json
{
  "intent": "extract pricing from vendor docs",
  "inputs": [{"type": "url", "value": "https://example.com/pricing"}],
  "constraints": {
    "budget": 0.05,
    "latency_ms": 500,
    "policy": ["no_pii"]
  },
  "proofs_required": ["attestation"]
}
```

## OFFER Payload
```json
{
  "offers": [
    {
      "route_id": "route-123",
      "capability": "summarize",
      "confidence": 0.89,
      "cost": {"unit": "usd", "estimate": 0.02},
      "latency": {"p50": "250ms"},
      "proofs": ["attestation"],
      "policy": ["no_pii"],
      "endpoint": "/mrp/negotiate"
    }
  ]
}
```

## NEGOTIATE Payload
```json
{
  "route_id": "route-123",
  "constraints": {
    "budget": 0.02,
    "policy": ["no_pii"]
  },
  "proofs": ["attestation"],
  "inputs": [{"type": "url", "value": "https://example.com/pricing"}]
}
```

## EXECUTE Payload
```json
{
  "route_id": "route-123",
  "inputs": [{"type": "url", "value": "https://example.com/pricing"}],
  "output_format": "markdown"
}
```

## EVIDENCE Payload
```json
{
  "route_id": "route-123",
  "outputs": [{"type": "markdown", "value": "..."}],
  "provenance": {
    "source_hashes": ["sha256:..."],
    "citations": ["https://example.com/pricing"],
    "timestamp": "2025-01-01T00:00:00Z"
  },
  "attestations": ["attestation"]
}
```

## PAYMENT INTENT (NEGOTIATE)
```json
{
  "payment_intent": {
    "intent_id": "pi_123",
    "currency": "usd",
    "max_amount": 0.05,
    "pricing_model": "per_request"
  }
}
```

## JOB_ACCEPTED Payload
```json
{
  "job_id": "job-123",
  "status": "accepted",
  "status_url": "/mrp/status/job-123"
}
```

## JOB_STATUS Payload
```json
{
  "job_id": "job-123",
  "status": "running",
  "progress": 0.6
}
```

## STREAM_CHUNK Payload
```json
{
  "stream_id": "stream-abc",
  "sequence": 1,
  "final": false,
  "chunk": {"type": "markdown", "value": "..."}
}
```

## ERROR Payload
```json
{
  "code": "MRP_RATE_LIMITED",
  "message": "Rate limit exceeded",
  "retryable": true,
  "retry_after_ms": 5000
}
```
