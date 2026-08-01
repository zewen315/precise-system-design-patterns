# Overview

## Use Linux

### Boot Process

- Firmware (BIOS/UEFI) → bootloader (GRUB) → kernel is loaded and decompressed → kernel mounts an initramfs (temporary root with just enough drivers to find the real disk) → kernel switches to the real root filesystem → kernel starts PID 1.
- What's the difference between BIOS and UEFI, and why did UEFI replace it (larger disks, faster boot, secure boot)?
- Why does the kernel need an initramfs at all instead of mounting the real root directly (it may need a driver, like an NVMe or LVM/RAID module, that lives *on* that root filesystem)?
- PID 1 is where `systemd.md` picks up.

### Kernel vs User Space

- Why is memory split into kernel space and user space, and why can't user space code touch hardware or other processes' memory directly?
- What are CPU privilege rings/modes (ring 0 = kernel, ring 3 = user on x86), and why is a system call the only sanctioned way to cross from one to the other?
- Why does a userspace crash (segfault) not take down the kernel, but a kernel panic takes down everything?

### Shell

(shell.md, shell-script.md)
- What is a shell
- Config shell
- Job control
- Shell-script usage
- `/usr/bin` ...

### Users & Groups

(users.md)

Users, groups, `/etc/passwd`/`/etc/shadow`, permissions, setuid/setgid, sudo vs su.

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

- A package manager tracks installed software, versions, and dependencies (dpkg/apt on Debian, rpm/dnf on Fedora/RHEL, pacman on Arch).
- What problem does dependency resolution solve, and why can it fail ("dependency hell" — two packages needing incompatible versions of the same library)?
- What's the difference between a package manager (apt/dnf) and a language-level one (pip/npm) — why do projects usually need both?

## System Resources

### CPU

Commands: `lscpu`, `nproc`, `top`, `htop`

How does `lscpu` works?
- `/sys/devices/system/cpu`
- `/proc/cpuinfo`

### Memory

(memory.md)

Commands: `free -h`, `ps`, `pmap`, `vmstat` — see `tools/memory.md`.
Others: `slabtop`, `dmesg`

- `/sys/devices/system/memory/`
- `/proc/meminfo`
- `/proc/<pid>/status`
- `/proc/<pid>/maps`, `/proc/<pid>/smaps`

Virtual memory, page tables, page faults, page cache, swap — see `memory.md`.

### Process

(process.md, thread.md)

- Lifecycle
- Signal
- Thread & synchronization (thread.md)

### Filesystem

(filesystem.md)

What is a file, inodes, file types, hardlink vs symlink, the Virtual File System (VFS), mounting, filesystem types — see `filesystem.md`.

### Containers

(containers.md)

Namespaces, cgroups, capabilities — the kernel primitives containers are built from.

### Network

(network.md)

Sockets, TCP connection lifecycle, blocking vs non-blocking I/O, `select`/`poll`/`epoll`, the NIC/DMA path — see `network.md`. CLI tools live in `tools/network.md`.

### Systemd

(systemd.md)