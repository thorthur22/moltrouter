# Authentication & Authorization

## Supported Auth Modes
- `bearer`: OAuth2/JWT/API key in `auth.token`.
- `mTLS`: mutual TLS at transport layer; `auth.type` set to `mTLS`.
- `signed`: detached signature of canonical envelope bytes.

## Auth Envelope
```json
{
  "auth": {
    "type": "bearer",
    "token": "<jwt-or-api-key>",
    "scopes": ["mrp:discover", "mrp:execute"],
    "issuer": "https://issuer.example.com",
    "signature": {
      "alg": "EdDSA",
      "key_id": "key-123",
      "value": "base64url..."
    }
  }
}
```

## Canonicalization (signed)
- Use RFC 8785 (JCS) canonical JSON for signing/verification.
- Exclude `auth.signature` from the signing payload.
- Signature is placed at `auth.signature` with `alg` and `key_id`.
- See `references/canonicalization.md`.

## Key Discovery
- Providers SHOULD expose `/.well-known/mrp-keys.json` with active public keys.
- Alternative discovery MAY use DID documents or certificate chains.

## Replay Protection
- Include `nonce` and `expires_at` in the envelope.
- Bind `msg_id` and `nonce` to the signature input.
- Receivers SHOULD reject expired or reused nonces within a replay window.

## Delegation
- Delegation SHOULD be represented as a signed grant object:
```json
{
  "delegation": {
    "delegator": "agent:moltbots/alpha",
    "delegate": "agent:moltbots/beta",
    "scope": ["mrp:execute"],
    "expires_at": "2025-01-01T00:05:00Z",
    "signature": {"alg": "EdDSA", "key_id": "key-123", "value": "base64url..."}
  }
}
```

## Errors
- `MRP_AUTH_REQUIRED`: missing or invalid auth.
- `MRP_SCOPE_DENIED`: auth valid but lacks required scope.
