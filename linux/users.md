# Users & Permissions

## Users & Groups

- Every process runs as a user (UID) and group (GID); root is UID 0.
- `/etc/passwd` — username, UID, GID, home dir, login shell (the password field is just `x`; the real hash lives in `/etc/shadow`).
- `/etc/shadow` — hashed passwords and password-aging policy, readable only by root.
- `/etc/group` — group name, GID, members.
- System/service accounts vs regular users — most distros reserve low UIDs (e.g. `< 1000`) for system accounts.
- `id`, `whoami`, `groups`, `getent passwd <user>`

## Authentication & Privilege Escalation

- `su` vs `sudo` — `su` switches identity fully and needs the *target* user's password; `sudo` runs a single command as another user and needs *your own* password, gated by policy in `/etc/sudoers`.
- What is PAM (Pluggable Authentication Modules), and how does it decouple "how a user authenticates" (password, key, MFA) from the applications that need authentication?
- `visudo` — why edit `/etc/sudoers` only through it (locks the file, validates syntax before saving, prevents a bad edit from locking everyone out)?
- setuid/setgid binaries — how does a binary like `passwd` let a regular user modify a root-owned file (`/etc/shadow`) safely, without giving them a root shell?

## File Permissions

- `rwx` for owner / group / other, and the octal representation (e.g. `755`, `644`).
- `chmod`, `chown`, `chgrp`
- Special bits:
  - setuid (`4000`) — run as the file's owner, not the invoking user.
  - setgid (`2000`) — run as the file's group; on a directory, new files inherit the directory's group.
  - sticky bit (`1000`, e.g. on `/tmp`) — in a shared, world-writable directory, only a file's owner (or root) can delete/rename it.
- `umask` — how does it subtract from the default permissions of newly created files/directories?
- ACLs (`getfacl` / `setfacl`) — what problem do they solve that the classic owner/group/other model can't (granting access to a specific extra user or group without changing ownership)?

## Interview / Practical Usage

- Why should a production service run as a dedicated non-root user instead of root?
- What's the security risk of a setuid-root binary, and why do modern systems prefer fine-grained Linux capabilities (`CAP_NET_BIND_SERVICE`, etc. — see `containers.md`) over full setuid-root?
- How would you debug "permission denied" — what's the checklist (file permissions, the directory's execute bit, ACLs, SELinux/AppArmor, ownership, mount options like `noexec`)?
- Why does a file need execute permission on every directory in its path (not just read) to be accessible at all?
