# Task Graph Routing (TGR)

## Graph Schema (DAG)
```json
{
  "graph_id": "graph-001",
  "nodes": [
    {
      "id": "n1",
      "capability": "extract",
      "inputs": [{"type": "url"}],
      "outputs": [{"type": "text"}],
      "constraints": {"policy": ["no_pii"]}
    },
    {
      "id": "n2",
      "capability": "summarize",
      "inputs": [{"type": "text"}],
      "outputs": [{"type": "markdown"}]
    }
  ],
  "edges": [
    {
      "from": "n1",
      "to": "n2",
      "mapping": {"text": "text"},
      "artifact": {"type": "artifact", "uri": "https://storage.example.com/n1.txt", "hash": "sha256:..."}
    }
  ],
  "constraints": {
    "max_cost": 0.05,
    "policy": ["no_pii"]
  },
  "delegation": [
    {"node_id": "n2", "delegate": "agent:moltbots/beta", "proof": "signed-grant"}
  }
}
```

## Routing Notes
- Nodes MUST declare inputs/outputs for automated wiring.
- Edges MAY declare transformation hints (e.g., format conversions).
- Graphs SHOULD carry per-node proofs and evidence aggregation rules.
