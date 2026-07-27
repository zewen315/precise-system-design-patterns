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
- 

execve()
- 

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

### How

Signal vs Realtime Signal

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

## Threads