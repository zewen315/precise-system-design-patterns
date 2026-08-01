# Disk Tools

## `du`

Disk usage of files/directories.

```bash
du -sh /var/log             # summarized, human-readable
du -h --max-depth=1 /         # per top-level directory
```

## `df`

Disk space usage per mounted filesystem.

```bash
df -h        # human-readable
df -i          # inode usage instead of blocks
```

## `fuser`

Identify which processes are using a file, directory, or port.

```bash
fuser -v /var/log/syslog     # who has this file open
fuser -k 8080/tcp               # kill whatever holds a port
```

## `stat`

Detailed metadata for a file (size, timestamps, inode, permissions).

```bash
stat file.txt
stat -f /     # filesystem-level info instead of file
```

## `iostat`

Per-device I/O throughput and utilization (part of `sysstat`).

```bash
iostat -x 1     # extended stats, every 1s
iostat -d 1        # device stats only
```

Key column: `%util` — how busy the device is.

## `iotop`

Live, top-like view of per-process disk I/O.

```bash
iotop -o     # only show processes actually doing I/O
```
