# Discovery & Registry

## Well-Known Capability Index
`/.well-known/mrp.json` returns a provider's capabilities and manifest location.

## Federated Registry
A registry MAY expose:
- `GET /mrp/registry/query?capability=summarize&policy=no_pii`
- Response includes providers, trust scores, and proofs.
- Registries SHOULD publish verification levels: `self_asserted`, `registry_attested`, `third_party_audited`.
- Support allow/deny lists and abuse reporting hooks.

Example response:
```json
{
  "mrp_version": "0.1",
  "next_page": "cursor-abc",
  "results": [
    {
      "provider": "service:clawdbots/summarize",
      "manifest_url": "https://example.com/mrp/manifest",
      "trust": {"score": 0.91, "proofs": ["attestation"], "level": "registry_attested"}
    }
  ]
}
```
