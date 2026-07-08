# Day 01 — Linux User Management & File Permissions

## 1. To view content 
ls 
## 2. User Management

### Types of Users
| Type | UID Range | Home Directory | Shell | Notes |
|---|---|---|---|---|
| **Root (Superuser)** | UID=0, GID=0 | `/root` | `/bin/bash` | Full system access |
| **System User** | 1–999 | Often `/no login` | `/sbin/nologin` or `/false` | Runs background services (e.g., `apache`) |
| **Local/Normal User** | 1000+ | `/home/username` | `/bin/bash` | Regular login user, locally stored |

### Core Security Concepts
- **Authorization** — what a user is allowed to do (permissions, group membership)
- **Authentication** — verifying identity (password/login check)
- **Auditing** — tracking user activity on the system

---

## 3. `useradd` Command
Used to create new users.

| Flag | Purpose |
|---|---|
| `-u` | Specify custom UID |
| `-c "comment"` | Add a comment/description |
| `-g` | Set primary group |
| `-G` | Set secondary/supplementary group(s) |
| `-m` | Create home directory |
| `-d` | Specify custom home directory location |
| `-s` | Define login shell |
| `-M` | Do **not** create a home directory |
| `-r` | Create a system user |
| **Skeleton (`/etc/skel`)** | Default files/folders copied into new user's home directory |

---

## 4. `usermod` Command
Used to modify existing users.

| Flag | Purpose |
|---|---|
| `-l new_username` | Rename an existing username |
| `-d new_home -m` | Change home directory path (and move contents) |
| `-L` | Lock account |
| `-U` | Unlock account |
| `-aG groupname` | Append user to a secondary group (without removing existing groups) |
| `-s` | Change login shell |
| `-u` | Change UID |
| `-g` | Change primary group |
| `-p` | Change password |

---

## 5. `userdel` Command
Used to delete users.

| Flag | Purpose |
|---|---|
| `-r` | Remove user along with home directory and mail spool |
| `-f` | Force deletion |

---

## 6. Password Management

### `passwd` command
| Flag | Purpose |
|---|---|
| `passwd username` | Set/change password for a user |
| `-l` | Lock password (temporarily disable login) |
| `-u` | Unlock password |
| `-S` | Check password status |
| `-d` | Delete password |

### `chage` — Change Age of Password
Manages password expiry policies.

| Flag | Purpose |
|---|---|
| `-l username` | Display password aging details |
| `-d` | Last password change date |
| `-m` | Minimum age before password can be changed |
| `-M` | Maximum age before password must be changed |
| `-W` | Warning period before expiry |
| `-E` | Account expiry date |
| `-I` | Inactive period after expiry |

To force password change on next login:
```bash
chage -d 0 username
```

---

## 7. Group Management
- A **group** is a collection of users that share the same access/permissions
- **Primary group** — default group assigned to a user
- **Secondary/Supplementary group** — extra groups a user is added to for additional access

### Commands
| Command | Purpose |
|---|---|
| `groupadd groupname` | Create a new group |
| `groupdel groupname` | Delete a group |
| `gpasswd groupname` | Set a group password |
| `gpasswd -a user group` | Add a user to a group |
| `gpasswd -d user group` | Remove a user from a group |
| `gpasswd -A user group` | Set a user as group admin |

### Ownership & Recursive Changes
```bash
chgrp groupname file        # change group ownership of a file
chgrp -R groupname dir/     # recursively change group for all files/folders inside a directory
```

---

## 8. `getent` — Get Entries from Name Service Switch
Used to query system databases.

```bash
getent group groupname      # check if a group exists
getent passwd username      # get user info
getent hosts google.com     # check host/DNS resolution
getent services ssh         # check service info
```

---

## 9. Important System Files

| File | Purpose |
|---|---|
| `/etc/passwd` | User account list — format: `username:x:UID:GID:comment:home_dir:shell` (the `x` is a password placeholder) |
| `/etc/shadow` | Encrypted password database — format includes username, encrypted password, last change, min/max age, warning, inactive period. **Only root can read this file.** |
| `/etc/group` | Group list — format: `group:x:GID:users` |
| `/etc/gshadow` | Group password/admin list — format: `group:encrypted_pass:admin_list:members` |
| `/etc/login.defs` | Defines default password policies (PASS_MAX_DAYS, PASS_MIN_DAYS, WARN_AGE, UID_MIN, UID_MAX range) |

---

## 10. File Permissions & Ownership

### Permission Basics
Every file has **3 levels of ownership**: User (owner), Group, Others

**File type indicators:**
| Symbol | Meaning |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `p` | Pipe |
| `s` | Socket |
| `c` | Character device |
| `b` | Block device |

**Permission types:**
- `r` (read) = 4
- `w` (write) = 2
- `x` (execute) = 1

### `chmod` — Change Mode/Permissions
```bash
chmod a+x file          # add execute for all (u=user, g=group, o=others, a=all)
chmod 755 file          # numeric form
chmod u=rwx,g=r,o=w file  # symbolic form, per category
chmod -R 755 project/   # recursive — applies to directory and all files/folders inside
```

### `chown` — Change Owner
```bash
chown owner:group file
chown -R owner:group dir/    # recursive
chown --reference=file1 file2   # copy ownership from one file to another
```
*Note: to change only the group, `chgrp` can also be used.*

### Directory Permissions (special meaning)
| Permission | Meaning for directories |
|---|---|
| `r` | Can list directory contents |
| `w` | Can create/delete files inside |
| `x` | Can enter (cd into) the directory |
| `w + x` | Required to create/delete files |
| `r + x` | Required to list and view files properly |

---

## 11. Special Permissions

### SUID (Set User ID)
- Normally, a program runs with the identity of the user who executes it
- With **SUID** set, the program runs with the identity of the file's **owner**, not the user executing it
- Example use case: `/etc/shadow` is only readable by root, but a program with SUID set (owned by root) lets a regular user change their password without directly accessing the file
- Numeric: adds **4** to the permission (e.g., `4755`)
- Symbolic: `chmod u+s file` / `chmod u-s file` to remove

### SGID (Set Group ID)
- Similar to SUID but applies at the group level
- On directories, files created inside inherit the group of the directory

### Sticky Bit
- Mainly used on directories
- Prevents users from deleting files they don't own, even if they have write access to the directory

---

## Key Takeaway
Today's focus was on **Linux user/group administration and permission management** — core skills for system administration and securing multi-user environments, directly relevant to infrastructure and DevOps work.
