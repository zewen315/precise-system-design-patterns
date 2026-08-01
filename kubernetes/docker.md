# Containers & Docker

> Questions to know about containers before discussing deployment in a system design or infra interview.

## Core Concepts

- What is a container, and how does it differ from a virtual machine (shared kernel vs full OS, startup time, isolation strength)?
- What Linux primitives make containers possible (namespaces for isolation, cgroups for resource limits — see `linux/process.md`)?
- What is a container image, and how do layers work (union filesystem, layer caching, why order matters in a Dockerfile)?
- What's the difference between an image and a container (image = template, container = running instance)?

## Docker Specifics

- What's the difference between `CMD` and `ENTRYPOINT` in a Dockerfile?
- What is a multi-stage build, and why does it reduce the final image size?
- How does Docker networking work by default (bridge network), and how does that differ from Pod networking in Kubernetes?
- What is a container registry, and how does image pulling/caching work?

## Trade-offs & Failure Modes

- Why is "a container isolates the process, not necessarily secures it" — what escapes are possible (kernel vulnerabilities, privileged containers)?
- What happens if a container exceeds its memory limit (OOMKilled)?
- What is image bloat, and why does it matter for deploy speed and attack surface?

## Interview Usage

- How would you explain the trade-off of many small containers vs fewer large VMs for a given workload?
- When does "we'd containerize this service" deserve more than a throwaway line in an interview?
