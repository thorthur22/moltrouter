# Types & Schema References

MRP manifests and payloads may refer to common types using `schema_ref`.

## Goal
Make discovery and wiring machine-automatic: if a node outputs `schema://mrp/types/text@1.0`, another node that accepts that type can be linked without guessing.

## Suggested Registry Convention
- `schema://mrp/types/<name>@<version>` for shared core types
- Providers MAY also publish custom types under their namespace:
  - `schema://service:clawdbots/summarize/types/<name>@<version>`

## Core Types (starter set)
- `text@1.0` (UTF-8 string)
- `url@1.0` (URL string)
- `json@1.0` (arbitrary JSON value)
- `markdown@1.0` (markdown string)
- `artifact@1.0` (see `schemas/types/artifact.schema.json`)

## Where to host
Providers SHOULD expose:
- `GET /mrp/schemas` (index)
- `GET /mrp/schemas/<path>` (actual JSON Schemas)

Agents MAY cache schemas by `$id`.
