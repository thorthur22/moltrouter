# Moltrouter Protocol (MRP)

This repository proposes **Moltrouter Protocol (MRP)**, a novel, agent-native communication and discovery protocol that enables autonomous agents (moltbots, clawdbots, openclaw agents, etc.) to traverse the internet without relying on human search interfaces. MRP is designed for **machine-first routing**, **capability discovery**, **task-scoped trust**, and **composable toolchains**.

---

## 1. Goals & Non-Goals

### Goals
- **Agent-native discovery**: Agents discover tools, data, and services directly via machine-readable routes.
- **Declarative capabilities**: Services describe capabilities in standardized, verifiable schemas.
- **Negotiated execution**: Agents and services negotiate intent, constraints, and budgets before execution.
- **Composable routing**: A task can be decomposed into a chain of services without human UX layers.
- **Safety by default**: Policy, provenance, and accountability built into the transport.

### Non-Goals
- Replacing the public web or HTTP.
- Centralized authority over discovery.
- Dependence on human search engines or keyword-based relevance.

---

## 2. Core Concepts

### 2.1 Agent-First Addressing (AFA)
AFA introduces a **route descriptor** that is more descriptive than a URL. It captures:

- **Intent** (what the agent wants done)
- **Capabilities** (what a node can do)
- **Constraints** (limits, safety policies)
- **Proofs** (identity, provenance, compliance)

Example:
```
mrp://capability/summarize?domain=finance&budget=0.02&format=md
```

### 2.2 Capability Manifests
Services publish `capability.json` (or `.mrp`) describing supported operations, schemas, costs, latency, and required proofs.

Example:
```json
{
  "capability": "summarize",
  "inputs": ["text", "url"],
  "outputs": ["markdown", "json"],
  "cost": {"unit": "usd", "estimate": 0.01},
  "latency": {"p50": "200ms"},
  "proofs": ["attestation", "rate_limit"],
  "policies": ["no_pii", "consent_required"]
}
```

### 2.3 Task Graph Routing (TGR)
Requests are routed as **graphs**, not linear calls. Each node is a tool/service/agent. Edges contain data flows, constraints, and proof requirements.

---

## 3. Protocol Stack

```
┌────────────────────────────┐
│     Intent Layer           │  (task graph, goals, policies)
├────────────────────────────┤
│     Capability Layer       │  (manifests, negotiation)
├────────────────────────────┤
│     Routing Layer          │  (AFA, DAG routing, scoring)
├────────────────────────────┤
│     Transport Layer        │  (HTTP/2, QUIC, libp2p)
└────────────────────────────┘
```

MRP operates **above transport**, allowing multiple transports.

---

## 4. Message Types

### 4.1 `HELLO`
Initial handshake: exchanges identities, supported schemas, and trust claims.

### 4.2 `DISCOVER`
Requests matching capabilities based on intent.

### 4.3 `OFFER`
Service returns candidate routes with cost, latency, and proofs.

### 4.4 `NEGOTIATE`
Agent and service agree on constraints, budget, safety policy.

### 4.5 `EXECUTE`
Calls a selected capability with structured inputs.

### 4.6 `EVIDENCE`
Returns execution evidence: provenance, attestation, citations.

---

## 5. Routing & Scoring

Route scoring considers:
- Capability match confidence
- Policy compliance
- Latency/cost tradeoff
- Trust score & proofs

Routing supports **multi-hop** delegation, allowing an agent to pass a task graph to other specialized agents.

---

## 6. Trust & Safety

MRP embeds a trust framework:
- **Proofs**: attestations, compliance logs, or zk-proofs
- **Policies**: enforced at negotiation
- **Audit trails**: execution evidence included

---

## 7. Example Flow

1. Agent sends `DISCOVER` for “extract pricing from vendor docs.”
2. Three services respond with `OFFER` manifests.
3. Agent chooses one and sends `NEGOTIATE` with budget + policy.
4. Service confirms and receives `EXECUTE` payload.
5. Service returns result with `EVIDENCE`.

---

## 8. Next Steps

Potential extensions:
- **Distributed registries** (DHT-based)
- **Capability marketplaces**
- **Federated trust scoring**
- **Dynamic policy negotiation**

---

## 9. Why This is Novel

MRP shifts away from human-centric browsing to a **machine-native routing layer** that treats services as composable capabilities, not web pages. It enables agents to traverse resources by declared function, verified proofs, and structured negotiation, rather than keyword search.

---

## 10. Agent Skill Package

This repo now includes an **agent skill** that packages the MRP specification into an actionable workflow with schemas and curl/npm-style usage examples. The skill lives at:

- `skills/moltrouter-protocol/`

It contains:
- `SKILL.md` with the workflow for applying MRP.
- `references/` with message envelopes, schemas, endpoints, auth, payments, errors, jobs/streaming, registry, task graphs, and rate limits.

This structure is intended to be bundled into a `.skill` file for distribution in agent ecosystems. The `.skill` bundle is generated as a build artifact and should not be committed as a binary in PRs; create it with:

```
zip -r dist/moltrouter-protocol.skill skills/moltrouter-protocol
```

MRP now specifies core operational gaps needed for real agent interoperability, including authentication, payment intents, error handling, async jobs, streaming, registry queries, and rate limits.

---

## 11. Required/Recommended Infrastructure

To make MRP usable in real deployments, the following infrastructure should be created:

### Required (minimum viable)
- **Capability manifest hosting**: service endpoints that expose `/mrp/manifest` and `/.well-known/mrp.json`.
- **MRP message endpoints**: `/mrp/hello`, `/mrp/discover`, `/mrp/negotiate`, `/mrp/execute`.
- **Identity + auth provider**: issue bearer tokens or signed envelopes for agents and services.
- **Evidence store**: retain execution evidence for provenance and auditability.

### Recommended (production-grade)
- **Federated registry**: registry nodes that serve `/mrp/registry/query` and share provider listings.
- **Async job service**: status, cancel, and streaming endpoints for long-running tasks.
- **Payments + metering**: payment intent validation, usage metering, and settlement logs.
- **Rate limiting + quotas**: enforce fair use and provide standard rate limit headers.
- **Trust anchor service**: attestation verification or proof registry for trust scoring.

---

## 12. DNS-less, P2P Federation Model

MRP does **not** require DNS or human-domain discovery. Registries can federate and communicate over P2P transports (e.g., libp2p), and agents can resolve service identities directly through registry lookups. This reduces context and token usage by avoiding human-facing discovery flows.

A single VPS with a stable IP can serve as a bootstrap registry node. Domains are optional: registries can manage naming and lookup at the protocol layer without requiring human DNS or branded domains. This allows lightweight SaaS-style deployment where the registry is reachable by IP and shares identity mappings across the federation.

---

## 13. Naming Ecosystem

- **Moltrouter**: protocol + routing engine
- **Moltbots**: autonomous agents using MRP
- **Clawdbots/Openclaw**: specialized agents and services

---

If you'd like, this can be expanded into:
- schema definitions
- reference implementation
- DHT discovery server
- SDK for agents
