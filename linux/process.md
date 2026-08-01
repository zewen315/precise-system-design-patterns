# Process

## Basics

### Resource

Resource of a process:

task_struct
- CPU context
- Memory
  - Code
  - Heap
  - Stack
  - ...
- Page table, VMA, mmap
- Open files
- cwd, root
- Credentials
- Signal
  - pending, blocked
  - signal handlers
- Scheduler
- PID
- IPC
- ...

Process Address Spac
- Environment Variables
- Command Line Arguments (argv)
- Stack
- Heap
- Data
- Code

Environment Variables

What is a PID?
- `cat /proc/sys/kernel/pid_max`
- `echo $$`
- PID 1 systemd

Process states
- stopped vs traced
- Interruptible vs uninterruptible
- zombie vs orphan, how to create zombie

```
include/linux/sched.h

TASK_RUNNING
TASK_INTERRUPTIBLE
TASK_UNINTERRUPTIBLE
TASK_STOPPED
TASK_TRACED
EXIT_ZOMBIE
EXIT_DEAD
```

### Syscalls

fork()
- Creates a new process descriptor (task_struct)
- Copies the CPU context
- Copies the virtual memory mappings
- Enables Copy-on-Write (COW)
- Duplicates the file descriptor table
- Copies process attributes
- Places the child in the scheduler's run queue

execve()
- Loads the new executable
- Destroys the old address space
- Creates a new address space
- Copies command-line arguments and environment variables
- Resets the CPU context
- Starts executing the new program

### System Calls (general)

- What is a system call, and why is it the only way for userspace to ask the kernel to do something privileged (access hardware, manage memory, talk to other processes)?
- How does a syscall actually transition from user mode to kernel mode (historically a software interrupt, `int 0x80`; modern x86-64 uses the faster `syscall`/`sysret` instructions)?
- What is a vDSO (virtual dynamic shared object), and why does it let a few syscalls (like `gettimeofday`) skip the mode-switch entirely by running in userspace?
- `strace` shows every syscall a process makes — see `tools/trace.md`.

### Lifecycle

Process Lifecycle

```
          fork()
             │
             ▼
            New
             │
             ▼
        Ready / Runnable
             │
          Scheduler
             ▼
          Running
         ↙        ↘
   Waiting      Preempted
(I/O, Sleep)        │
      │             │
      └─────────────┘
             │
             ▼
         Running
             │
             ▼
          Zombie
             │
             ▼
          Reaped
```

## Signals

### Why

- Signals are the kernel's mechanism for asynchronous notification — telling a process about an event (user interrupt, hardware exception, another process's request) without the process having to poll for it.
- Why can a signal handler run at almost any point in a program's execution, and why does that make writing a *safe* handler hard (async-signal-safety — only a small, specific set of functions are safe to call inside one)?

### How

- Each process has a disposition per signal number: the default action, ignored, or a custom handler (`sigaction`).
- Pending vs blocked: a signal can be blocked (masked) so delivery is deferred, but — for standard signals — it isn't queued if it arrives again while still blocked.
- `kill(pid, sig)` doesn't necessarily kill anything — it just sends a signal; the name is historical.

Signal vs Realtime Signal
- Standard signals (1–31) are not queued: if the same signal arrives multiple times while blocked, only one delivery happens once it's unblocked.
- Realtime signals (`SIGRTMIN`–`SIGRTMAX`) are queued, can carry a small payload (`sigqueue`), and are delivered in order.

### Commonly used

| Signal | Number* | Typical Source | Default Action | Catchable? | Description |
|---------|---------|----------------|----------------|------------|-------------|
| `SIGHUP` | 1 | Terminal disconnect, `kill -HUP` | Terminate | ✔ | Originally indicated a terminal hangup; commonly used to reload configuration. |
| `SIGINT` | 2 | `Ctrl+C` | Terminate | ✔ | Interrupts a foreground process. |
| `SIGQUIT` | 3 | `Ctrl+\` | Core Dump | ✔ | Terminates the process and generates a core dump. |
| `SIGABRT` | 6 | `abort()` | Core Dump | ✔ | Indicates an abnormal termination requested by the program itself. |
| `SIGKILL` | 9 | `kill -9` | Terminate | ❌ | Immediately kills a process. Cannot be caught, blocked, or ignored. |
| `SIGSEGV` | 11 | Invalid memory access | Core Dump | ✔ | Sent when a process performs an illegal memory access (Segmentation Fault). |
| `SIGPIPE` | 13 | Writing to a closed pipe/socket | Terminate | ✔ | Prevents processes from writing indefinitely to a broken pipe. |
| `SIGALRM` | 14 | `alarm()` | Terminate | ✔ | Delivered when a timer created by `alarm()` expires. |
| `SIGTERM` | 15 | `kill` | Terminate | ✔ | Requests graceful termination. Applications usually perform cleanup before exiting. |
| `SIGCHLD` | 17* | Child process exits | Ignore | ✔ | Sent to a parent process when one of its children terminates or stops. |
| `SIGCONT` | 18* | `kill -CONT` | Continue | ✔ | Resumes a stopped process. |
| `SIGSTOP` | 19* | `kill -STOP` | Stop | ❌ | Immediately stops a process. Cannot be caught or ignored. |
| `SIGTSTP` | 20* | `Ctrl+Z` | Stop | ✔ | Stops a foreground process. Unlike `SIGSTOP`, it can be handled or ignored. |

## ELF Executables

- What is ELF (Executable and Linkable Format), and why is it the standard binary format for executables, shared libraries, and object files on Linux?
- What are the main pieces of an ELF file (the header, program headers describing segments to load, section headers, symbol table)?
- What's the difference between a statically linked and a dynamically linked binary, and why do most binaries dynamically link libc?
- What does the dynamic linker/loader (`ld.so`) do at program startup, before `main()` ever runs (resolving and mapping shared library dependencies)?
- `ldd ./binary` — list a binary's shared library dependencies.
- `readelf -h ./binary`, `file ./binary` — inspect ELF metadata.
