# Threads & Synchronization

## Process vs Thread

- What's the actual kernel-level difference between a process and a thread on Linux (both are `task_struct`s; threads of the same process share an address space, file descriptor table, and signal handlers, but each gets its own stack, registers, and TID)?
- What does `clone()` do, and how do both `fork()` and `pthread_create()` boil down to it with different flag combinations (`CLONE_VM`, `CLONE_FILES`, `CLONE_SIGHAND`, ...)?
- Why is a context switch between threads of the same process cheaper than between two processes (no address-space/page-table switch, no TLB flush)?
- What's the difference between a PID and a TID, and why does `ps -T` or `top -H` show multiple rows for a single process?

## Threading Models

- User-level threads vs kernel-level threads (1:1, N:1, M:N models) — which model do Linux `pthread`s use (1:1 — each thread is its own schedulable kernel entity)?
- What's a green thread / goroutine, and why do languages like Go run their own M:N scheduler on top of OS threads instead of using 1:1 directly?

## Concurrency Problems

- What is a race condition, and what makes it especially hard to reproduce/debug (timing-dependent, often only surfacing under load)?
- What is a critical section, and why must access to shared, mutable state be serialized?
- What is a deadlock, and what are the four necessary conditions for one (mutual exclusion, hold-and-wait, no preemption, circular wait)?
- What's the difference between a deadlock and a livelock?

## Synchronization Primitives

- What is a mutex, and how does it differ from a binary semaphore (a mutex has ownership — only the thread that locked it can unlock it; a semaphore doesn't)?
- What is a counting semaphore used for (limiting concurrent access to N units of a resource, e.g. a connection pool)?
- What is a condition variable, and why is it always paired with a mutex (to avoid the lost-wakeup race between checking a condition and waiting on it)?
- What is a spinlock, and when is busy-waiting actually preferable to sleeping (very short critical sections, avoiding context-switch overhead)?
- What is a read-write lock, and why does it help when reads vastly outnumber writes?

## futex

- What is a futex ("fast userspace mutex"), and why does it let uncontended locking happen entirely in userspace, only calling into the kernel when there's actual contention?
- How do `pthread_mutex`, semaphores, and most userspace locking primitives end up built on top of `futex()` under the hood?

## Interview / Practical Usage

- When would you choose multiple processes over multiple threads for a workload (fault isolation, avoiding a language-level global lock, simpler reasoning) vs threads (cheap sharing of large in-memory state)?
- How would you debug a deadlocked multi-threaded process (`gdb`'s `thread apply all bt`, or a language-level equivalent like `jstack`/`py-spy`)?
- Why doesn't adding more threads always increase throughput (Amdahl's law, lock contention, cache-line bouncing between cores)?
