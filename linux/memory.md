# Memory Management

> Concepts behind the numbers `free`/`vmstat`/`pmap` show — see `tools/memory.md` for the commands themselves.

## Virtual Memory

- What is virtual memory, and why does every process get its own private address space instead of touching physical RAM directly (isolation between processes, a simpler uniform address layout, overcommit)?
- What is a page, and what's the typical page size on x86-64 (4KB, with larger "huge pages" available)?
- What is a page table, and how does the MMU (memory management unit) use it to translate a virtual address into a physical one on every memory access?
- What is a TLB (translation lookaside buffer), and why does it exist (caching recent page-table translations, since walking the table on every single access would be far too slow)?
- What is a page fault, and what's the difference between a minor fault (the page exists but isn't mapped into this process yet, e.g. lazy allocation or a shared library) and a major fault (the page must actually be read from disk/swap)?

## Address Space Layout

- What lives in a process's virtual address space (text/code, initialized/uninitialized data, heap, memory-mapped region, stack) — and which direction do the heap and stack grow toward each other?
- What is `mmap`, and how does it let a file be mapped directly into memory instead of read via repeated `read()` calls?
- What is copy-on-write (COW), and how does `fork()` use it to make process creation cheap by sharing pages until either process writes to one (see `process.md`)?

## Page Cache

- What is the page cache, and why does Linux deliberately use "free" RAM to cache file data rather than leaving it idle?
- Why does `free -h`'s `available` column matter more than `free` — the kernel will reclaim cache under memory pressure, so cached memory isn't really unavailable to applications?
- What's the difference between a clean page (matches disk, can be dropped instantly) and a dirty page (must be written back before it can be reclaimed)?
- `sync`, `echo 3 > /proc/sys/vm/drop_caches` — what do they actually do, and why is manually dropping caches almost never something you should do in production?

## Swap

- What is swap, and why doesn't having swap in use necessarily mean the system is "out of memory" (the kernel may swap out cold, rarely-touched pages to keep more page cache around)?
- What is `swappiness`, and how does it tune the kernel's preference for swapping anonymous memory vs reclaiming page cache first?
- Why is swapping especially damaging for latency-sensitive services (disk/SSD I/O is orders of magnitude slower than RAM, so a swapped-in page stalls the whole request)?

## OOM Killer

- What triggers the OOM (out-of-memory) killer, and how does it choose a victim process (`oom_score`, adjustable via `oom_score_adj`)?
- How do you find evidence of an OOM kill after the fact (`dmesg | grep -i oom`, `journalctl -k`)?
- How does a cgroup-scoped OOM kill (a container hitting its `MemoryMax`) differ from a system-wide OOM kill (only that cgroup's processes are candidates, not the whole host)?

## Interview / Practical Usage

- A process's RSS keeps growing over time — how do you tell a real memory leak apart from expected cache growth or fragmentation?
- How would you size a container's memory limit for a JVM/Node process, given the process needs headroom beyond just its heap (thread stacks, native allocations, GC overhead)?
- Why can two processes' memory numbers in `ps` overlap and still not double-count total system usage (shared libraries, shared/copy-on-write pages)?
