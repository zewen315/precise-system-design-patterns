# Distributed Tracing

> Questions to know about tracing before a system design or SRE interview.

## Core Concepts

- What problem does distributed tracing solve that logs and metrics don't (following a single request across many services)?
- What is a span, and what is a trace (a trace is a tree/DAG of spans)?
- How is trace context propagated across service boundaries (trace ID/span ID in headers, e.g., W3C Trace Context)?
- What is OpenTelemetry, and what problem does it solve (a vendor-neutral instrumentation standard)?

## Practical Usage

- What is sampling in tracing (head-based vs tail-based), and why do you sample given the cost of tracing every request at scale?
- How would you use a trace to diagnose "this request is slow" — what does the waterfall view show you (which span/service ate the time)?
- What's the difference between tracing and profiling?

## Interview Usage

- If an interviewer asks "how would you find why a specific request was slow across 10 microservices," what's your answer?
- How would you propose adding tracing to a design without it becoming prohibitively expensive at scale?
