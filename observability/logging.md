# Logging

> Questions to know about log aggregation before a system design or SRE interview.

## Core Concepts

- Why do logs matter even when you have metrics — what question do logs answer that metrics can't (exact request context, error detail)?
- What is structured logging (JSON logs with fields) vs unstructured text logs, and why does structure matter at scale?
- What is a log level (DEBUG/INFO/WARN/ERROR), and how do you decide what to log at each?

## Architecture

- Why can't you just read log files on each server at scale — what problem does centralized log aggregation solve?
- How does a typical log pipeline work (agent on each host → shipping → indexing/storage → query UI, e.g., Fluentd/Filebeat → Kafka → Elasticsearch/Loki)?
- What's the difference between the ELK stack's approach (full-text indexing in Elasticsearch) and Loki's approach (indexing only labels, not full text) — what's the cost/performance trade-off?
- What is log sampling, and why might you drop most successful-request logs while keeping all errors?

## Interview Usage

- How would you correlate a single user request across logs, metrics, and traces (a shared correlation/trace ID)?
- What retention policy would you propose for logs, and why (cost vs compliance/debugging needs)?
