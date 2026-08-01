# Observability Overview

> Questions to know about the fundamentals of observability before a system design or SRE interview.

## Core Concepts

- What problem does observability solve that traditional monitoring doesn't (understanding unknown-unknowns vs watching known dashboards)?
- What are the three pillars of observability (metrics, logs, traces), and what question does each answer best?
- What's the difference between monitoring and observability?
- What's the difference between a symptom-based alert and a cause-based alert, and why do most mature teams alert on symptoms (e.g., user-facing error rate) rather than causes?

## Interview Usage

- If asked "how would you monitor this system" in a system design interview, what's a concise, high-signal answer (key metrics, SLOs, alerting, dashboards)?
- What metrics would you propose for a typical read-heavy web service — the "four golden signals" (latency, traffic, errors, saturation)?
