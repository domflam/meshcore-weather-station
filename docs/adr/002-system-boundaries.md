# ADR-002: MeshCore transport with GCP integration boundary

- Status: Accepted
- Date: 2026-10-03

## Context

The remote site has LoRa/MeshCore coverage but no requirement for direct Internet connectivity. The club website and mobile application are administered independently.

## Decision

Use MeshCore only as the remote telemetry transport. An Internet-connected gateway converts telemetry to HTTPS/JSON and posts it to a Google Cloud endpoint. Cloud Run provides the integration API and BigQuery stores history.

## Consequences

Website/mobile developers consume a conventional REST API and need no MeshCore knowledge. Cloud credentials remain at the gateway rather than on the remote station.
