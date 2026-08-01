# Tracing & Profiling Tools

## `strace`

Trace syscalls made by a process.

```bash
strace -p <pid>                      # attach to a running process
strace -f -e trace=network ./app     # follow children, filter by category
strace -c ./app                      # summarize syscall counts/time
```

## `ltrace`

Trace library (userspace) calls, e.g. libc functions.

```bash
ltrace -p <pid>
ltrace -c ./app     # summary of library calls
```

## `perf`

Linux profiler using hardware/software performance counters.

```bash
perf top             # live view of hottest functions
perf stat ./app        # summary counters (cycles, instructions, cache misses)
perf record -g ./app     # record call graph for later analysis
perf report                # view perf.data recorded above
```

## `bpftrace`

High-level tracing language on top of eBPF for dynamic tracing/probing.

```bash
bpftrace -e 'tracepoint:syscalls:sys_enter_open { printf("%s %s\n", comm, str(args->filename)); }'
bpftrace -l 'tracepoint:syscalls:*'   # list available tracepoints
```
