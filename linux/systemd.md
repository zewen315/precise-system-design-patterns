# Systemd

## Basics

- systemd is PID 1 — the first userspace process the kernel starts, and the ancestor of every other process.
- `/sbin/init` is symlinked to `/usr/lib/systemd/systemd` on most modern distros.
- `ps -p 1` — confirm systemd is PID 1.
- Why does every process tree need a PID 1: it reaps orphaned/zombie processes (see `process.md`) and is the entry point that brings the rest of the system up.

## Units

- A "unit" is systemd's basic object — everything (services, sockets, mounts, timers, targets) is a unit.
- Common unit types: `.service`, `.socket`, `.timer`, `.mount`, `.target`, `.device`.
- Unit files live in `/usr/lib/systemd/system` (packages), `/etc/systemd/system` (admin overrides/local units), `/run/systemd/system` (runtime, generated).
- `systemctl cat <unit>` — show the effective unit file, merged across all three locations.

### Service Unit Anatomy

```ini
[Unit]
Description=My App
After=network.target
Requires=postgresql.service

[Service]
Type=simple
ExecStart=/usr/bin/myapp
Restart=on-failure
RestartSec=5
User=myapp

[Install]
WantedBy=multi-user.target
```

- `[Unit]` — description and dependency ordering (`After`, `Before`, `Requires`, `Wants`).
- `[Service]` — how to start/stop/restart it, and as which user.
- `[Install]` — which target pulls this unit in when it's enabled.

### Service Types

- `Type=simple` — the process itself is the service (default).
- `Type=forking` — the process forks and the parent exits; systemd tracks the child.
- `Type=oneshot` — runs to completion, doesn't stay running (good for setup scripts).
- `Type=notify` — the service tells systemd it's ready via `sd_notify()`.

## Dependencies & Ordering

- `Requires=` vs `Wants=` — `Requires` fails the unit if the dependency fails to start; `Wants` is a soft dependency that doesn't block on failure.
- `After=`/`Before=` — ordering only; it doesn't imply a dependency, so a unit can be "after" another without requiring it to succeed.
- Why do you usually need both a `Requires`/`Wants` *and* an `After` together (one says "must exist", the other says "must come first")?

## Targets

- A target is a synchronization point / named group of units — the systemd equivalent of a SysV runlevel.
- Common targets: `multi-user.target` (normal boot, no GUI), `graphical.target`, `rescue.target`, `reboot.target`.
- `systemctl get-default` / `systemctl set-default multi-user.target`
- `systemctl isolate rescue.target` — switch to a different target immediately.

## systemctl Commands

```bash
systemctl status nginx           # current state, recent logs, cgroup
systemctl start|stop|restart nginx
systemctl enable nginx           # symlink into the target's .wants/, so it starts on boot
systemctl enable --now nginx     # enable and start in one step
systemctl daemon-reload          # re-read unit files after editing them
systemctl list-units --failed    # what's broken
systemctl mask nginx             # hard-disable, even against manual start
```

- Why does editing a unit file require `daemon-reload` before the change takes effect?
- What's the difference between `enable` (creates a boot-time symlink) and `start` (starts it right now) — why do people forget one and get confused later?
- What does `mask` do that `disable` doesn't (symlinks the unit to `/dev/null`, blocking even a manual `start`)?

## Journald

- systemd's logging component; captures stdout/stderr of every service plus kernel messages.
- Queried with `journalctl` (see `tools/journal.md`).
- `journalctl -u <service>` shows only that unit's logs — no need to know or manage a log file path.

## Socket Activation

- What is socket activation, and how does it let systemd start a service lazily on first connection instead of unconditionally at boot?
- How does a `.socket` unit hand off an already-listening file descriptor to the `.service` it activates?

## cgroups Integration

- Every service unit gets its own cgroup — how does that let systemd reliably track and kill a service's entire process tree (`systemctl stop` won't leave orphaned children behind)?
- `CPUQuota=`, `MemoryMax=` in a unit file — how does systemd let you resource-limit a service without reaching for a separate cgroup tool (see `containers.md`)?

## Failure Modes & Interview Usage

- A service is `enabled` but isn't running after a reboot — what's your debugging checklist (`systemctl status`, `journalctl -u`, a failed upstream `Requires`/`After` dependency)?
- `Restart=on-failure` + `RestartSec=` + `StartLimitBurst=` — how do you avoid a crash-looping service hammering a downstream dependency?
- Why is systemd controversial (its monolithic scope vs the traditional Unix "do one thing well" philosophy) — what's the argument on each side?
