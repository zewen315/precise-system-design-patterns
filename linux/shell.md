# Shell

## What is a Shell

What is a shell

What are commands of a shell?
- Built-in Commands (cd: chdir)
  - ```type cd ```
  - ```help```
  - ```command -V cd```
- External Commands

How to check the current shell?
  - ```echo $SHELL```
  - ```echo $0```
  - ```ps -p $$```
  - ```cat /etc/shells```

How does a shell start?
  - ssh (parent process is ```sshd```)
  - login (parent process is ```getty```)

shell vs terminal vs tty
  - A Terminal (or Terminal Emulator) is the application that provides a window for text-based interaction.
  - A TTY (Teletype) is a character device managed by the operating system that represents an interactive terminal.
  - A Shell is a command interpreter.

                Terminal / SSH / Docker
                         │
                         ▼
              Kernel creates a PTY
                         │
                  /dev/pts/0 (slave)
                         │
          dup2(slave, stdin/stdout/stderr)
                         │
                         ▼
                      exec(bash)
                         │
                         ▼
             bash only calls read(0)/write(1)
                         │
                         ▼
                   glibc → syscall → Kernel

Type of shell
- bash vs zsh

## Config Shell

export vs source

.bash_profile vs .bashrc
- One-time login initialization
- Configuration for every Bash session
