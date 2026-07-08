# Day 01 — Linux User Management & File Permissions

## What I learned today:

### To view content 
- `ls` listing content
- `ls -l` list with info
- `ls -lh` list with info in human readable form
- `ls -lt` list ( sorted based on time )
- `ls -lZ` view hidden files 

### User Management

**Types of users:**
- **Root (superuser)** — UID=0, GID=0, home directory `/root`, shell `/bin/bash`, full system access
- **System user** — UID range 1–999, home often `/no login`, shell `/sbin/nologin` or `/false`, runs background services (e.g., `apache`)
- **Local/normal user** — UID 1000+, home directory `/home/username`, shell `/bin/bash`, regular login user

**Core security concepts:**
- **Authorization** — what a user is allowed to do (permissions, group membership)
- **Authentication** — verifying identity (password/login check)
- **Auditing** — tracking user activity on the system

### `useradd` Command
Used to create new users.
- `-u` specify custom UID
- `-c "comment"` add a comment/description
- `-g` set primary group
- `-G` set secondary/supplementary group(s)
- `-m` create home directory
- `-d` specify custom home directory location
- `-s` define login shell
- `-M` do **not** create a home directory
- `-r` create a system user
- **Skeleton (`/etc/skel`)** — default files/folders copied into new user's home directory

### `usermod` Command
Used to modify existing users.
- `-l new_username` rename an existing username
- `-d new_home -m` change home directory path (and move contents)
- `-L` lock account
- `-U` unlock account
- `-aG groupname` append user to a secondary group (without removing existing groups)
- `-s` change login shell
- `-u` change UID
- `-g` change primary group
- `-p` change password

### `userdel` Command
Used to delete users.
- `-r` remove user along with home directory and mail spool
- `-f` force deletion

### Password Management

**`passwd` command:**
- `passwd username` set/change password for a user
- `-l` lock password (temporarily disable login)
- `-u` unlock password
- `-S` check password status
- `-d` delete password

**`chage` — Change Age of Password**
Manages password expiry policies.
- `-l username` display password aging details
- `-d` last password change date
- `-m` minimum age before password can be changed
- `-M` maximum age before password must be changed
- `-W` warning period before expiry
- `-E` account expiry date
- `-I` inactive period after expiry
- To force password change on next login: `chage -d 0 username`

### Group Management
- A **group** is a collection of users that share the same access/permissions
- **Primary group** — default group assigned to a user
- **Secondary/supplementary group** — extra groups a user is added to for additional access

**Commands:**
- `groupadd groupname` create a new group
- `groupdel groupname` delete a group
- `gpasswd groupname` set a group password
- `gpasswd -a user group` add a user to a group
- `gpasswd -d user group` remove a user from a group
- `gpasswd -A user group` set a user as group admin

**Ownership & recursive changes:**
```bash
chgrp groupname file        # change group ownership of a file
chgrp -R groupname dir/     # recursively change group for all files/folders inside a directory
```

### `getent` — Get Entries from Name Service Switch
Used to query system databases.
```bash
getent group groupname      # check if a group exists
getent passwd username      # get user info
getent hosts google.com     # check host/DNS resolution
getent services ssh         # check service info
```

### Important System Files
- `/etc/passwd` — user account list, format: `username:x:UID:GID:comment:home_dir:shell` (the `x` is a password placeholder)
- `/etc/shadow` — encrypted password database, includes username, encrypted password, last change, min/max age, warning, inactive period. **Only root can read this file.**
- `/etc/group` — group list, format: `group:x:GID:users`
- `/etc/gshadow` — group password/admin list, format: `group:encrypted_pass:admin_list:members`
- `/etc/login.defs` — defines default password policies (PASS_MAX_DAYS, PASS_MIN_DAYS, WARN_AGE, UID_MIN, UID_MAX range)

### File Permissions & Ownership

Every file has 3 levels of ownership: **user (owner), group, others**

**File type indicators:**
- `-` regular file
- `d` directory
- `l` symbolic link
- `p` pipe
- `s` socket
- `c` character device
- `b` block device

**Permission values:**
- `r` (read) = 4
- `w` (write) = 2
- `x` (execute) = 1

**`chmod` — change mode/permissions:**
```bash
chmod a+x file          # add execute for all (u=user, g=group, o=others, a=all)
chmod 755 file          # numeric form
chmod u=rwx,g=r,o=w file  # symbolic form, per category
chmod -R 755 project/   # recursive — applies to directory and all files/folders inside
```

**`chown` — change owner:**
```bash
chown owner:group file
chown -R owner:group dir/    # recursive
chown --reference=file1 file2   # copy ownership from one file to another
```
*Note: to change only the group, `chgrp` can also be used.*

**Directory permissions (special meaning):**
- `r` — can list directory contents
- `w` — can create/delete files inside
- `x` — can enter (cd into) the directory
- `w + x` — required to create/delete files
- `r + x` — required to list and view files properly

### Special Permissions

**SUID (Set User ID):**
- Normally, a program runs with the identity of the user who executes it
- With SUID set, the program runs with the identity of the file's **owner**, not the user executing it
- Example: `/etc/shadow` is only readable by root, but a program with SUID set (owned by root) lets a regular user change their password without directly accessing the file
- Numeric: adds **4** to the permission (e.g., `4755`)
- Symbolic: `chmod u+s file` / `chmod u-s file` to remove

**SGID (Set Group ID):**
- Similar to SUID but applies at the group level
- On directories, files created inside inherit the group of the directory

**Sticky bit:**
- Mainly used on directories
- Prevents users from deleting files they don't own, even if they have write access to the directory

---
**Takeaway:** Focused on Linux user/group administration and permission management today — core skills for securing and managing multi-user systems, directly relevant to infrastructure and DevOps work.

