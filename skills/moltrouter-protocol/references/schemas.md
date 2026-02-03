# MRP Schemas

## Canonical JSON Schemas
Each message type SHOULD have a versioned JSON Schema (`$id` per type + version).
Implementations MUST validate against these schemas to claim conformance.

## Capability Manifest (`application/mrp-manifest+json`)
```json
{
  "capability_id": "capability:summarize",
  "capability": "summarize",
  "version": "1.0",
  "tags": ["text:summarization", "format:markdown"],
  "inputs": [
    {"type": "text", "schema_ref": "schema://mrp/types/text@1.0"},
    {"type": "url", "schema_ref": "schema://mrp/types/url@1.0"}
  ],
  "outputs": [
    {"type": "markdown", "schema_ref": "schema://mrp/types/markdown@1.0"}
  ],
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

## Artifact Reference
```json
{
  "type": "artifact",
  "uri": "https://storage.example.com/artifacts/abc",
  "hash": "sha256:...",
  "size": 1048576,
  "mime": "application/pdf"
}
```

## DISCOVER Payload
```json
{
  "intent": "extract pricing from vendor docs",
  "inputs": [
    {"type": "url", "value": "https://example.com/pricing"}
  ],
  "constraints": {
    "max_cost": 0.05,
    "max_latency_ms": 500,
    "data_residency": "us",
    "policy": ["no_pii", "no_training"]
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
      "risk": {
        "data_retention_days": 0,
        "training_use": "none",
        "subprocessors": []
      },
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
    "max_cost": 0.02,
    "policy": ["no_pii"],
    "allowed_domains": ["example.com"]
  },
  "proofs": ["attestation"],
  "inputs": [{"type": "url", "value": "https://example.com/pricing"}]
}
```

## EXECUTE Payload
```json
{
  "route_id": "route-123",
  "inputs": [
    {"type": "url", "value": "https://example.com/pricing"},
    {"type": "artifact", "uri": "https://storage.example.com/input.pdf", "hash": "sha256:..."}
  ],
  "output_format": "markdown"
}
```

## EVIDENCE Payload
```json
{
  "route_id": "route-123",
  "outputs": [
    {"type": "markdown", "value": "..."},
    {"type": "artifact", "uri": "https://storage.example.com/output.md", "hash": "sha256:..."}
  ],
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
