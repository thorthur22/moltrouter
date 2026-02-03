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
    "issuer": "https://issuer.example.com"
  }
}
```

## Canonicalization (signed)
- Canonicalize the envelope as UTF-8 JSON with sorted keys.
- Exclude `auth.signature` from the signing payload.
- Signature is placed at `auth.signature` with `alg` and `key_id`.

## Errors
- `MRP_AUTH_REQUIRED`: missing or invalid auth.
- `MRP_SCOPE_DENIED`: auth valid but lacks required scope.
