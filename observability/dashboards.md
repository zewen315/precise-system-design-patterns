# Dashboards (Grafana)

> Questions to know about visualizing metrics before a system design or SRE interview.

## Core Concepts

- What role does Grafana play relative to Prometheus (Prometheus stores/queries data, Grafana visualizes it) — is Grafana itself a data store?
- What data sources can Grafana query besides Prometheus (Loki, Elasticsearch, CloudWatch, SQL databases)?
- What's the difference between a panel, a dashboard, and a data source?

## Practical Usage

- How would you build a dashboard around the four golden signals (latency, traffic, errors, saturation) for a service?
- What's the difference between a dashboard used for real-time debugging vs one used for long-term capacity planning?
- How does Grafana alerting differ from Alertmanager, and why might a team use both?

## Interview Usage

- If an interviewer asks "what would the on-call dashboard for this service look like," what would you sketch out?
- Why is "we'd add a Grafana dashboard" often a throwaway line in a system design interview — what's the deeper point the interviewer is actually checking for?
