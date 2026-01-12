## Overview
This document describes how Cowrie honeypot logs flow through Loki and how they can be visualized in Grafana. It also explains why this setup is chosen over alternatives and key design decisions.

The pipeline is lightweight, suitable for a single Raspberry Pi SOC, and avoids heavy containerized stacks like ELK.

## Architecture
1. Local logs: Cowrie writes JSON logs to the filesystem.
2. Collection: Promtail tails logs and forwards them to Loki with labels (job, service_name, filename).
3. Storage: Loki stores logs for 7–14 days, optimized for label-based queries.
4. Visualization: Grafana dashboards query Loki for alerts and analysis.

## Loki Configuration
- Loki is a log aggregation system designed to work like Prometheus but for logs.
- It indexes only labels instead of full text, making it fast and lightweight.
- Ideal for JSON logs from Cowrie with low hardware resources.

## Why Loki vs ELK
- Loki is lightweight, label-indexed, and integrates natively with Grafana. Ideal for Raspberry Pi and small SOC deployments.
- ELK provides full-text search and advanced analytics but requires more CPU, RAM, and storage, making it overkill for local, low-volume logging.

Decision: Loki offers the right cost/performance trade-off for this project.

## Labels & Querying

Labels allow structured queries without indexing full text.

Typical labels:
- `job` → service name (e.g., cowrie)
- `filename` → log source
- `sensor` → host identifier

This approach enables fast filtering and efficient storage.

## Running Loki
Recommended via systemd:
```
sudo systemctl enable loki
sudo systemctl start loki
```
- Ensure Promtail is configured to forward the correct files and labels.

## Summary
Cowrie → Promtail → Loki → Grafana
- Efficient, local-first, low-cost
- Label-based logs for fast filtering
- Short-term retention to suit Raspberry Pi resources
- Easy integration with alerts and dashboards

