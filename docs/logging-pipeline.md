## Overview
This document describes how Cowrie honeypot logs flow through Loki and how they can be visualized in Grafana. It also explains why this setup is chosen over alternatives and key design decisions.

The pipeline is lightweight, suitable for a single Raspberry Pi SOC, and avoids heavy containerized stacks like ELK.

**Pipeline flow:**
```
Cowrie JSON logs → Loki (log collection & indexing) → Grafana (visualization & alerting)
```

## Loki Configuration
- Loki is a log aggregation system designed to work like Prometheus but for logs.
- It indexes only labels instead of full text, making it fast and lightweight.
- Ideal for JSON logs from Cowrie with low hardware resources.
