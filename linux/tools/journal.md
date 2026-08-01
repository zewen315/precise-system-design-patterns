# Journal / Log Tools

## `journalctl`

Query the systemd journal.

```bash
journalctl -u nginx.service      # logs for a specific unit
journalctl -f                      # follow, like tail -f
journalctl --since "1 hour ago"
journalctl -p err                    # filter by priority
```

## `tail`

Print the end of a file, optionally following new lines.

```bash
tail -f /var/log/syslog     # follow in real time
tail -n 100 file.log           # last 100 lines
```

## `dmesg`

Kernel ring buffer messages (boot, hardware, driver, OOM events).

```bash
dmesg -T             # human-readable timestamps
dmesg -w               # follow new messages
dmesg | grep -i oom       # find OOM killer events
```
