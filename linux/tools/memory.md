# Memory Tools

## `free`

Quick snapshot of system memory and swap usage.

```bash
free -h          # human-readable
free -h -s 2       # refresh every 2s
```

`available` (not `free`) is the realistic number of usable memory, since Linux caches aggressively.

## `vmstat`

Virtual memory, process, CPU, and I/O statistics sampled over time.

```bash
vmstat 1        # report every 1s
vmstat -a 1       # active/inactive memory
vmstat -s          # summary of memory counters since boot
```

Key columns: `r` (runnable procs), `si`/`so` (swap in/out), `wa` (I/O wait).
