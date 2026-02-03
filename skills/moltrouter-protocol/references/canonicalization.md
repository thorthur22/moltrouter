# Canonicalization (for `auth.type = signed`)

To make signature verification interoperable, MRP signed envelopes SHOULD use a single canonical JSON serialization.

## Recommendation
Use **JSON Canonicalization Scheme (JCS)** as specified in **RFC 8785**.

## Rules
- Serialize the envelope as canonical UTF-8 JSON per RFC 8785.
- Exclude the `auth.signature` field from the signing input (treat it as empty / absent).
- Sign the resulting bytes using the declared algorithm (e.g., Ed25519).
- Place the detached signature in `auth.signature`.

## Notes
- Avoid "sort keys" ad-hoc rules; different implementations disagree on number formatting, unicode escaping, etc.
- If you cannot implement full JCS, document the exact deviations and consider treating signatures as advisory only.
