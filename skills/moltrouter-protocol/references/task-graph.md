# Task Graph Routing (TGR)

## Graph Schema (DAG)
```json
{
  "graph_id": "graph-001",
  "nodes": [
    {"id": "n1", "capability": "extract", "inputs": ["url"], "outputs": ["text"]},
    {"id": "n2", "capability": "summarize", "inputs": ["text"], "outputs": ["markdown"]}
  ],
  "edges": [
    {"from": "n1", "to": "n2", "mapping": {"text": "text"}}
  ],
  "constraints": {
    "budget": 0.05,
    "policy": ["no_pii"]
  }
}
```

## Routing Notes
- Nodes MUST declare inputs/outputs for automated wiring.
- Edges MAY declare transformation hints (e.g., format conversions).
