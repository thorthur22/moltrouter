# MRP Endpoints + CLI Examples

## Well-Known Discovery
Expose a capability index at:
- `/.well-known/mrp.json`

Example response:
```json
{
  "mrp_version": "0.1",
  "capabilities": ["summarize", "extract", "classify"],
  "manifest_url": "/mrp/manifest"
}
```

## HTTP Endpoints
- `GET /mrp/manifest` -> capability manifest
- `POST /mrp/hello`
- `POST /mrp/discover`
- `POST /mrp/negotiate`
- `POST /mrp/execute`
- `POST /mrp/artifacts/init` -> pre-signed upload URL negotiation (recommended)
- `GET /mrp/artifacts/{artifact_id}` -> download/redirect (recommended)
- `GET /mrp/status/{job_id}`
- `POST /mrp/cancel`
- `GET /mrp/stream/{stream_id}`
- `GET /mrp/registry/query`
- `GET /.well-known/mrp-keys.json`

All requests and responses use `Content-Type: application/mrp+json`.

## Curl Examples
### HELLO
```bash
curl -s https://example.com/mrp/hello \
  -H 'Content-Type: application/mrp+json' \
  -d '{"mrp_version":"0.1","msg_id":"uuid","msg_type":"HELLO","timestamp":"2025-01-01T00:00:00Z","sender":{"id":"agent:moltbots/alpha"},"receiver":{"id":"service:clawdbots/summarize"},"payload":{"schemas":["0.1"],"proofs":["attestation"]}}'
```

### DISCOVER
```bash
curl -s https://example.com/mrp/discover \
  -H 'Content-Type: application/mrp+json' \
  -d '{"mrp_version":"0.1","msg_id":"uuid","msg_type":"DISCOVER","timestamp":"2025-01-01T00:00:00Z","sender":{"id":"agent:moltbots/alpha"},"payload":{"intent":"extract pricing","inputs":[{"type":"url","value":"https://example.com/pricing"}],"constraints":{"budget":0.05,"policy":["no_pii"]},"proofs_required":["attestation"]}}'
```

### EXECUTE
```bash
curl -s https://example.com/mrp/execute \
  -H 'Content-Type: application/mrp+json' \
  -d '{"mrp_version":"0.1","msg_id":"uuid","msg_type":"EXECUTE","in_reply_to":"uuid","timestamp":"2025-01-01T00:00:00Z","sender":{"id":"agent:moltbots/alpha"},"receiver":{"id":"service:clawdbots/summarize"},"payload":{"route_id":"route-123","inputs":[{"type":"url","value":"https://example.com/pricing"}],"output_format":"markdown"}}'
```

### JOB STATUS
```bash
curl -s https://example.com/mrp/status/job-123 \
  -H 'Content-Type: application/mrp+json'
```

### REGISTRY QUERY
```bash
curl -s 'https://example.com/mrp/registry/query?capability=summarize&policy=no_pii' \
  -H 'Content-Type: application/mrp+json'
```

## npm-style Usage (Pseudo-CLI)
These are example CLI patterns for an agent tooling package.
```bash
npx mrp discover --intent "extract pricing" --url https://example.com/pricing --budget 0.05
npx mrp negotiate --route route-123 --budget 0.02 --policy no_pii
npx mrp execute --route route-123 --format markdown
```
