# Neural Retina (Neural Lens Agent Interface)

This skill enables OpenClaw to act as the "Neural Retina" interface for the Neural Prism platform. It performs zero-latency web audits against provided documents (PDFs) or URLs, bypassing search caches to verify live facts.

## Capabilities

1.  **Live Verification (Zero-Lag):**
    - Navigates directly to target URLs using a headless browser (via `browser` tool).
    - Extracts _current_ text and takes a timestamped screenshot (evidence).
    - Compares live content against claims found in a local PDF or text.

2.  **Audit Report Generation:**
    - Generates a JSON or Markdown report matching the Neural Lens metric format.
    - Scores: `PASS`, `FAIL`, or `INCONCLUSIVE`.
    - Evidence: Includes screenshot path and exact text snippet.

## Metrics Format (JSON - DAG Compatible)

To support Neural Lens visualization, the report follows a graph structure:

```json
{
  "audit_id": "uuid",
  "timestamp": "ISO-8601",
  "nodes": [
    { "id": "doc_root", "type": "document", "label": "filename.pdf" },
    { "id": "claim_1", "type": "claim", "label": "Pricing is $10/mo" },
    {
      "id": "verify_1",
      "type": "verification",
      "label": "Live Check: example.com",
      "status": "PASS",
      "latency": "0ms"
    },
    { "id": "evidence_1", "type": "evidence", "label": "Screenshot", "path": "evidence.png" }
  ],
  "edges": [
    { "source": "doc_root", "target": "claim_1" },
    { "source": "claim_1", "target": "verify_1" },
    { "source": "verify_1", "target": "evidence_1" }
  ]
}
```
