# SLIs, SLOs, and Alerting

> Questions to know about reliability targets and on-call practice before an SRE-flavored interview.

## Core Concepts

- What's the difference between an SLI (indicator), an SLO (objective), and an SLA (agreement with consequences)?
- What is an error budget, and how does it let a team balance shipping velocity against reliability?
- Why do most SLOs use percentiles (p99 latency) instead of averages, and what does an average hide?

## Alerting Practice

- What's the difference between alerting on a cause (CPU > 80%) vs a symptom (error rate/latency SLO burn)?
- What is alert fatigue, and how does over-alerting on low-signal conditions cause real incidents to get missed?
- What is a multi-window, multi-burn-rate alert, and why does it catch both fast and slow SLO burns without being too noisy?

## Interview Usage

- If asked to define an SLO for a service in your design, what would you propose and why (e.g., 99.9% of requests < 300ms)?
- How would you explain the trade-off of tightening an SLO (more reliable, slower feature velocity)?
