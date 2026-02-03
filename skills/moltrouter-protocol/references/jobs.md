# Jobs & Streaming

## JOB_ACCEPTED Payload
```json
{
  "job_id": "job-123",
  "status": "accepted",
  "status_url": "/mrp/status/job-123",
  "estimated_ms": 2000
}
```

## JOB_STATUS Payload
```json
{
  "job_id": "job-123",
  "status": "running",
  "progress": 0.6,
  "eta_ms": 800
}
```

## JOB_COMPLETE Payload
When complete, return `EVIDENCE` with `job_id` and final outputs.

## STREAM_CHUNK Payload
```json
{
  "stream_id": "stream-abc",
  "sequence": 4,
  "final": false,
  "chunk": {"type": "markdown", "value": "..."}
}
```

## Cancelation
- `POST /mrp/cancel` with `job_id`.
- Return `JOB_STATUS` with `status = "canceled"`.
