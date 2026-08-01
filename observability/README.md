# Observability Notes

> Notes on metrics, logs, traces, and reliability targets — the tooling and vocabulary that come up when an interviewer asks "how would you monitor this?"

## What You'll Find

* The three pillars: metrics, logs, traces
* Prometheus (metrics collection & alerting)
* Grafana (dashboards)
* Log aggregation
* Distributed tracing
* SLIs, SLOs, and alerting practice

## Philosophy

Each file is written as the important questions to know before an interview, matching the format used in [components/](../components/README.md):

* What question does this tool/concept actually answer that the others don't?
* How does it work under the hood?
* What's the cost/trade-off of using it at scale (cardinality, storage, sampling)?
* What's a high-signal answer if an interviewer asks "how would you monitor this system"?

## Repository Structure

```text
observability/
├── overview.md
├── metrics.md
├── dashboards.md
├── logging.md
├── tracing.md
└── alerting-slo.md
```
