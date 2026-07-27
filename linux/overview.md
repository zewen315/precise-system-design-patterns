# Overview

## Use Linux

### Boot Process

### Kernel vs User Space

### Shell

(shell.md, shell-script.md)
- What is a shell
- Config shell
- Job control
- Shell-script usage
- `/usr/bin` ...

### Users & Groups

Understand user, group, passwd

`/etc/passwd`

### Files

```bash
/
├── bin
├── sbin
├── usr
├── lib

├── etc

├── home
├── root

├── boot

├── dev
├── proc
├── sys

├── media
├── mnt

├── opt
├── srv

├── tmp
├── run
└── var
```

### Packages

## System Resources

### CPU

Commands: `lscpu`, `nproc`, `top`, `htop`

How does `lscpu` works?
- `/sys/devices/system/cpu`
- `/proc/cpuinfo`

### Memory

Commands: `free -h`, `ps`, `pmap`, `vmstat`,
Others: `slabtop`, `dmesg`

- `/sys/devices/system/memory/`
- `/proc/meminfo`
- `/proc/<pid>/status`
- `/proc/<pid>/maps`, `/proc/<pid>/smaps`

### Process

(process.md)

- Lifecycle
- Signal
- Thread

### Filesystem

What is a file?
- 

File types:
- 

Hardlink vs Softlink
- 

Virtual File System (VFS)

### Network

### Systemd