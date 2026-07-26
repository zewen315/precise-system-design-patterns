# Linux Notes

> A collection of notes for understanding how Linux works under the hood.

Linux is one of the most important pieces of software every engineer interacts with, yet many of its core concepts remain a black box.

This repository is a personal knowledge base that documents the fundamental ideas behind Linux—from process management and virtual memory to shells, signals, networking, and filesystems.

The goal is not to explain every kernel implementation detail.

The goal is to build a solid mental model of how Linux works.

---

## What You'll Find

Topics covered include:

* Processes & Threads
* Scheduling
* Memory Management
* Virtual Memory
* Page Tables
* System Calls
* ELF Executables
* Signals
* Shell
* Job Control
* TTY & PTY
* Pipes & IPC
* File Systems
* Networking
* epoll & Event Loops
* Synchronization
* Containers & Namespaces
* cgroups
* Security

Each topic contains concise notes, diagrams, examples, and references that focus on understanding the underlying concepts rather than memorizing commands.

---

## Philosophy

This repository aims to answer questions like:

* What really happens after calling `fork()`?
* Why does `exec()` replace the current process?
* How does a shell execute a command?
* Why does `Ctrl-C` send `SIGINT`?
* What happens when pressing `Ctrl-Z`?
* How does a TTY work?
* What is the difference between a process and a thread?
* Why do page tables exist?
* How does `epoll` scale?
* How do containers isolate processes?
* What actually happens during system boot?

Instead of treating Linux as a collection of commands, this repository focuses on understanding the mechanisms behind the operating system.

---

## Repository Structure

```text
linux-notes/
├── processes/
├── scheduling/
├── memory/
├── system-calls/
├── shell/
├── signals/
├── tty/
├── filesystem/
├── networking/
├── synchronization/
├── containers/
└── references/
```

Each directory contains topic-oriented notes, diagrams, code snippets, and links to additional resources.

---

## Intended Audience

This repository is intended for:

* Software Engineers
* Site Reliability Engineers
* Backend Engineers
* Systems Engineers
* Students learning operating systems
* Anyone interested in understanding Linux internals

Some familiarity with Linux commands is helpful, but no prior kernel development experience is required.

---

## References

The notes are compiled from official documentation, books, source code, conference talks, and personal learning.

Recommended references include:

* *Operating Systems: Three Easy Pieces (OSTEP)*
* *Linux Kernel Development*
* *The Linux Programming Interface*
* *Computer Systems: A Programmer's Perspective*
* Linux Kernel Source Code
* POSIX Specifications
* `man` Pages

---

## Why This Repository?

Linux is everywhere—from laptops and servers to cloud infrastructure and containers.

Understanding how it works makes it easier to debug production systems, design reliable software, and reason about performance.

This repository serves as a long-term reference for learning and revisiting Linux internals.
