# Artifacts (Large/Binary Inputs & Outputs)

MRP payloads should avoid embedding large/binary blobs in JSON. Instead, exchange **artifact references** with hashes.

## Artifact Reference
See `schemas/types/artifact.schema.json`.

## Minimal Upload Flow (recommended)
A provider MAY expose an artifact service:
- `POST /mrp/artifacts/init` → returns an upload URL and required headers
- `GET /mrp/artifacts/{artifact_id}` → returns/downloads the artifact (or redirects)

### INIT request
```json
{
  "name": "input.pdf",
  "mime": "application/pdf",
  "size": 1048576,
  "hash": "sha256:..."
}
```

### INIT response
```json
{
  "artifact_id": "art-123",
  "upload": {
    "method": "PUT",
    "url": "https://storage.example.com/presigned/...",
    "headers": {"Content-Type": "application/pdf"}
  },
  "artifact": {
    "type": "artifact",
    "uri": "https://provider.example.com/mrp/artifacts/art-123",
    "hash": "sha256:...",
    "size": 1048576,
    "mime": "application/pdf"
  }
}
```

## Verification
- Providers SHOULD verify uploaded content hash matches the declared `hash`.
- Agents SHOULD verify returned artifact hashes before trusting downstream.

## Retention & Policy
Providers SHOULD declare retention defaults in OFFER.risk (e.g. `data_retention_days`).
