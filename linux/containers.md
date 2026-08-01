# Containers: Namespaces & cgroups

> The kernel primitives that make a "container" possible. See `system-design-patterns/kubernetes/docker.md` for the container/image-level concepts built on top of these.

## Namespaces

- What is a namespace, and how does it give a process a private view of a global kernel resource?
- What namespace types exist, and what does each isolate?
  - `PID` — process IDs (a process can be PID 1 inside its namespace, invisible from outside it).
  - `NET` — network interfaces, routing tables, ports.
  - `MNT` — mount points (its own view of the filesystem tree).
  - `UTS` — hostname and domain name.
  - `IPC` — System V IPC objects and POSIX message queues.
  - `USER` — UID/GID mappings (root inside the namespace can map to an unprivileged UID outside it).
  - `CGROUP` — hides the cgroup hierarchy above the container's own root.
- Why does a process inside a PID namespace see itself as PID 1, while the host still sees its real, different PID?
- What does `unshare` do, and how can you experiment with namespaces directly without a full container runtime?
- What's left un-isolated by namespaces alone (the kernel itself is shared, along with physical devices and, until `time` namespaces, the system clock)?

## cgroups (Control Groups)

- What problem do cgroups solve that namespaces don't — namespaces isolate *what a process can see*, cgroups limit *how much of a resource it can use*?
- What resources can a cgroup limit or account for (CPU, memory, block I/O, number of PIDs, network via `net_cls`)?
- What's the difference between cgroups v1 (a separate hierarchy per resource controller) and cgroups v2 (a single unified hierarchy)?
- How does `memory.max` trigger an OOM kill scoped to just that cgroup instead of the whole host (see `memory.md`)?
- How does `cpu.max` throttle a process without killing it — what does "CPU throttled" look like from inside a container that's hitting its quota?

## Putting It Together: What Is a Container?

- A container is not a kernel object — there's no `container` syscall. It's namespaces + cgroups + a filesystem (usually a layered/union filesystem image) + restricted syscalls/capabilities, orchestrated by a runtime (runc, containerd).
- Why does this explain why containers share the host kernel while VMs don't — and what's the security/isolation implication of that?

## Capabilities & Security

- What are Linux capabilities, and how do they split root's traditionally all-or-nothing power into fine-grained privileges (`CAP_NET_BIND_SERVICE`, `CAP_SYS_ADMIN`, ...)?
- Why do container runtimes drop most capabilities by default, and what does running a container `--privileged` actually disable (essentially all of the isolation above)?
- What is seccomp, and how does it restrict which syscalls a process is allowed to make at all, regardless of capabilities?
- What's the difference between capabilities, seccomp, and mandatory access control (SELinux/AppArmor) — how do they layer together for defense in depth?

## Interview / Practical Usage

- How would you explain "why is a container less isolated than a VM" using namespaces/cgroups/shared-kernel as the answer?
- How would you debug a container being OOM-killed vs the underlying host being OOM-killed?
- Why might two containers on the same host be able to see each other's processes if PID namespaces aren't configured correctly (e.g. `--pid=host`)?
