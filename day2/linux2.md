# Day 02 — ACL, Sudo, SELinux, Links, File System & Cron Jobs

## What I learned today:

### ACL (Access Control List)
- Traditional Linux permissions only allow one owner, one group, and others — limited
- ACL allows extra users/groups to be given specific permissions beyond the traditional model

- `getfacl file` — view ACL entries on a file
- `setfacl -m u:username:rwx file` — modify ACL, add permission for a specific user
- `setfacl -m g:groupname:rw file` — modify ACL for a specific group
- `setfacl -x u:username file` — remove a specific ACL entry
- `setfacl -b file` — remove all ACL entries
- Default ACL — applies automatically to new files created inside a directory
- `setfacl -d -m u:username:rwx directory` — set default ACL on existing files in a directory
- `setfacl -k directory` — remove default ACL from a directory
- ACL Mask — defines the maximum effective permissions allowed

---

### Sudo Privileges
- Used to give a local user root privileges
- Two methods:
  - Add the user to the `wheel` group
  - Edit the sudoers config file (`/etc/sudoers`)
- Direct editing of `/etc/sudoers` is not recommended — use `visudo` instead, since it validates syntax before saving
- Sudoers file syntax: `user host=(as_user) commands`
  - Example: `root ALL=(ALL) ALL`
  - Example: `rahul ALL=(root) /usr/bin/useradd` — rahul can only run the `useradd` command as root
- `NOPASSWD:ALL` — user won't be prompted for a password
- For groups: `%groupname ALL=(ALL) ALL`
- `/etc/sudoers.d/` — separate directory for individual sudoers config entries
- Always check the command path (e.g., `which systemctl`) before adding it to a sudoers rule

---

### UMASK
- UMASK removes certain permission bits from the default permissions when a new file/directory is created
- Default permissions:
  - Files: `666`
  - Directories: `777`
- Common umask value: `022`
  - `666 - 022 = 644` (file default with umask applied)
  - `777 - 022 = 755` (directory default with umask applied)
- `umask` — view current umask value
- `umask -S` — view umask in symbolic form
- Running `umask 002` directly — temporary change (only for current session)
- Permanent change:
  - User-specific: edit `~/.bashrc` or `~/.bash_profile`
  - System-wide: edit `/etc/bashrc`

---

### SELinux (Security-Enhanced Linux) — Basics Only
- Main goal: **least privilege principle** — a process should get only as much permission as it actually needs
- Adds a kernel-level extra security layer on top of normal Linux permissions

**DAC vs MAC:**
- **DAC (Discretionary Access Control)** — traditional permissions; owner/group/others decide access
- **MAC (Mandatory Access Control)** — SELinux's model; the system's security policy decides access, not the file owner

**SELinux Modes:**
- **Enforcing** — SELinux policy is actively checked; rule violations are denied and logged
- **Permissive** — violations are not blocked, only logged as a warning (useful for debugging)
- **Disabled** — SELinux is completely off

**Checking/changing mode:**
- `getenforce` — check current mode
- `sestatus` — detailed status
- `setenforce 0` — temporarily switch to permissive
- `setenforce 1` — temporarily switch to enforcing
- Permanent change: edit `/etc/selinux/config` (`SELINUX=enforcing` is the common default)

**Policy types:**
- **Targeted** — default SELinux policy; protects only selected processes
- **MLS (Multi-Level Security)** — advanced, more strict

**SELinux Context format:** `user:role:type:level`
- Example: `unconfined_u:object_r:admin_home_t:s0`
- **SELinux user** — separate from the Linux user (e.g., `system_u`, `user_u`)
- **Role** — associated with files/objects
- **Type/label** — defines what a process/file is allowed to do (e.g., `httpd_sys_content_t`, `user_home_t`)

**Context commands:**
- `chcon -t type file` — temporarily change context of a file
- `restorecon file` — restore the file's original/default context

---

### Hard Link vs Soft Link (Symbolic Link)

**Hard Link:**
- Another name for the same file — points to the same inode
- `ls -li` — shows the inode number
- If one copy is modified, the change reflects in the other too (since they share the same data)
- Cannot cross filesystems
- Cannot be created for a directory
- Deleting one link doesn't delete the actual data as long as another link still exists
- Command: `ln original_file new_hardlink`

**Soft Link (Symbolic Link):**
- Works like a shortcut — has its own separate inode
- Stores the **path** of the original file, not the same inode
- If the original file is deleted, the soft link breaks (becomes a "dangling link")
- Command: `ln -s original_file new_softlink`

---

### File System in Linux
- Used by the OS to manage, store, and retrieve files and directories on a storage device
- Keeps track of where data is stored on disk
- Without a file system, the OS wouldn't know where a file starts/ends, or which disk sectors are free or occupied
- Linux follows a **tree structure**, with `/` as the root directory — everything starts here

**Filesystem Hierarchy:**
- `/` — root, everything starts here
- `/bin` — binary executables for users
- `/sbin` — system administration binaries
- `/boot` — files required to boot the system
- `/dev` — device files (e.g., `/dev/sda`)
- `/etc` — configuration files
- `/home` — user home directories
- `/root` — home directory of the root user
- `/lib` and `/lib64` — shared libraries
- `/media` — automatically mounted removable devices
- `/mnt` — temporary mount point
- `/sys` — kernel information
- `/run` — runtime information
- `/tmp` — temporary files
- `/usr` — contains applications, libraries, commands, etc.
- `/var` — variable data (data that changes frequently)
- `/var/log` — log files

---

### Cron Jobs & Scheduling
- Used to run a command/script automatically at a specific time
- **Types:**
  - **cron** — runs a job repeatedly on a schedule
  - **at** — runs a job only once

**Cron basics:**
- Cron is a background service/daemon that executes scheduled tasks repeatedly according to a predefined schedule
- Daemon name: `crond`
- User cron jobs stored at: `/var/spool/cron/username`
- System-wide cron jobs stored at: `/etc/crontab`

**Managing cron jobs:**
- `crontab -l` — view current user's cron jobs
- `crontab -u username -l` — view another user's cron jobs
- `crontab -e` — edit current user's cron jobs
- `crontab -u username -e` — edit another user's cron jobs

**Cron syntax:**
![Cron Syntax Diagram](cronimage.jpeg)

**Examples:**
- `*/5 * * * *` — every 5 minutes
- `0 * * * *` — every hour, at minute 0
- `*/10 * * * *` — every 10 minutes
- `0 9 * * 1` — every Monday at 9 AM
- `0 8 1 * *` — 1st of every month at 8 AM
- `0 12 25 12 *` — 12 PM on the 25th of December every year

**Special characters:**
- `*` — means "every"
- `,` — multiple values (e.g., `1,3,5`)
- `-` — range (e.g., `3-5`)
- `/` — step value (e.g., `*/5`)

---
**Takeaway:** Covered advanced Linux administration topics today — fine-grained permission control with ACL, granting elevated access safely with sudo, controlling default permissions with umask, kernel-level security with SELinux, the difference between hard and soft links, how the Linux file system is structured, and automating tasks with cron job scheduling.
