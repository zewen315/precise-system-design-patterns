# Metrics (Prometheus)

> Questions to know about metrics collection before a system design or SRE interview.

## Core Concepts

- What is a metric, and what are the common types (counter, gauge, histogram, summary)?
- Why is a counter monotonically increasing, and how do you compute a rate from it (`rate()`/`irate()`)?
- What's the difference between a histogram and a summary, and why do histograms let you aggregate percentiles across instances while summaries don't?

## Prometheus Architecture

- Why does Prometheus use a pull model (scraping a `/metrics` endpoint) instead of a push model, and what's the trade-off?
- What is an exporter, and why do you need one for systems that don't natively expose Prometheus metrics?
- How does Prometheus discover targets to scrape (static config, service discovery)?
- What is cardinality, and why can a high-cardinality label (e.g., user ID) blow up Prometheus's memory/storage?
- How does Prometheus store data (local TSDB), and why is long-term retention/global querying usually handled by a separate system (Thanos/Cortex/Mimir)?

## Alerting

- What is Alertmanager, and how does it relate to Prometheus (Prometheus evaluates rules and fires alerts; Alertmanager routes/dedupes/silences them)?
- What's the difference between an alerting rule and a recording rule?

## Interview Usage

- How would you instrument a service to expose the four golden signals as Prometheus metrics?
- Why would you avoid putting a raw request ID or user ID into a Prometheus label?
