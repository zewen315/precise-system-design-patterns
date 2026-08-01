# Process Tools

## `ps`

Snapshot of currently running processes.

```bash
ps aux                  # all processes, BSD-style, with %cpu/%mem
ps -ef                  # all processes, full format with PPID
ps -p <pid> -o pid,ppid,stat,cmd
ps --forest             # show parent/child tree
```

## `lsof`

List open files — and by extension sockets, since "everything is a file".

```bash
lsof -p <pid>              # files opened by a process
lsof -i :8080              # process listening on/using port 8080
lsof -i tcp -sTCP:LISTEN   # all listening TCP sockets
lsof +D /var/log           # files open under a directory
```

## `pidstat`

Per-process resource usage over time (part of `sysstat`).

```bash
pidstat 1          # CPU usage per process, every 1s
pidstat -d 1        # per-process disk I/O
pidstat -r 1          # per-process memory / page faults
pidstat -p <pid> 1
```

## `sar`

System Activity Reporter — historical/periodic system-wide stats (part of `sysstat`).

```bash
sar -u 1 5     # CPU utilization, 5 samples 1s apart
sar -r 1        # memory utilization
sar -n DEV 1     # network device stats
sar -q 1           # load average / run queue
```

## `ulimit`

Shell built-in to view/set per-process resource limits.

```bash
ulimit -a        # show all limits
ulimit -n          # max open file descriptors
ulimit -n 4096       # raise open-file limit for this shell/session
ulimit -u              # max user processes
```
