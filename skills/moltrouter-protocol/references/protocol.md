# Moltrouter Protocol (MRP) Core Reference

## Overview
MRP is a machine-native protocol for agent discovery, negotiation, and execution. It layers on top of existing transports and standardizes how agents describe intent, discover capabilities, negotiate constraints, and exchange evidence.

## Content Types
- `application/mrp+json` for all MRP message envelopes.
- `application/mrp-manifest+json` for capability manifests.
- `application/mrp-batch+json` for batch or streaming envelope chunks.

## Versioning & Compatibility
- `mrp_version` follows `MAJOR.MINOR`.
- Services MUST accept the same major version and SHOULD accept minor versions within the same major.
- A service MAY reject incompatible versions with `ERROR` code `MRP_VERSION_UNSUPPORTED`.

## Addressing
- `mrp://capability/<capability>?intent=<intent>&budget=<amount>&format=<format>`
- `mrp://route/<route-id>` for route references.

## Message Envelope
All messages use the same envelope fields:
```json
{
  "mrp_version": "0.1",
  "msg_id": "uuid",
  "msg_type": "HELLO|DISCOVER|OFFER|NEGOTIATE|EXECUTE|EVIDENCE|JOB_ACCEPTED|JOB_STATUS|STREAM_CHUNK|ERROR",
  "timestamp": "2025-01-01T00:00:00Z",
  "sender": {"id": "agent:moltbots/alpha", "proofs": ["attestation"]},
  "receiver": {"id": "service:clawdbots/summarize"},
  "trace": {"root": "uuid", "parent": "uuid"},
  "auth": {"type": "bearer", "token": "..."},
  "idempotency_key": "uuid",
  "payload": {}
}
```

## Authentication & Authorization
- Supported `auth.type`: `bearer`, `mTLS`, `signed`.
- `signed` auth uses detached signatures over the canonicalized envelope.
- Services MAY require scopes in `auth` and return `MRP_AUTH_REQUIRED` on failure.

## Core Flow
1. **HELLO**: exchange supported schemas and trust claims.
2. **DISCOVER**: declare intent + constraints.
3. **OFFER**: return candidate capabilities/routes with cost and proofs.
4. **NEGOTIATE**: agree on budget, policies, and proofs.
5. **EXECUTE**: invoke selected capability.
6. **EVIDENCE**: return outputs with provenance and citations.

## Async + Streaming
- Long-running work returns `JOB_ACCEPTED` with `job_id` and `status_url`.
- Streaming uses chunked envelopes with `stream_id` and `sequence` fields in payload.

## Routing
- Represent routes as DAGs where nodes are capabilities and edges are data constraints.
- Route selection considers capability match, policy compliance, cost/latency, and trust.

## Trust + Evidence
- Require explicit proofs (attestations, compliance logs, zk-proofs) in negotiation.
- Return evidence with output provenance, hash references, and execution metadata.

## Payments
- Use `payment_intent` in `NEGOTIATE` to pre-authorize spend.
- Confirm final charge in `EVIDENCE` with usage and settlement metadata.

## Errors & Retries
- `ERROR` payload contains `code`, `message`, `retryable`, and `retry_after_ms`.
- Idempotent requests SHOULD include `idempotency_key` and reuse it on retry.
