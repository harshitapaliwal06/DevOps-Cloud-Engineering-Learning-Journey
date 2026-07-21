# Day 7 — Package Management, grep, find, xargs & sed

## What I learned today:

### Package Management — Basics

**What is a Package?**
- Not just an archive — a package is software that contains archive + metadata (info that tells how to install it correctly, and all info needed to install it)

**RPM (Red Hat Package Manager)**
- Low-level package management system
- Does not automatically search online or resolve dependencies
- Software that knows how to open and install RPM packages (like a "worker" that opens and installs)
- RPM cannot install/download dependencies on its own

**DNF (does dependency resolution for RPM)**
- DNF is the manager that fetches the right box (package) and tells RPM to install
- Example flow:
  - I want to install `apache`
  - DNF searches online (repo)
  - Finds `httpd.rpm`
  - Downloads it
  - Finds dependencies
  - Downloads them
  - Hands everything to RPM
  - RPM installs the package
  - Done

**DNF vs RPM:**
- DNF — high-level package manager
- RPM database — contains info about installed packages: version, release, name, etc.
- Modern RHEL repos use **BaseOS** and **AppStream**
- DNF = "dandified yum" (newer version of yum)

**What happens when installing (`dnf install git`):**
1. DNF receives the command to install
2. DNF contacts the repo
3. Repo provides metadata
4. DNF reads dependencies
5. If dependencies aren't present, DNF resolves them
6. DNF downloads the RPM packages
7. Hands them to RPM
8. RPM begins install — copies files, updates the database

---

### Repository Basics
- A repository contains: **Packages** (archive + metadata) and **Metadata** (dependencies, libraries, signatures)
- Configured in `/etc/yum.repos.d/`
- Contains repo config files ending in `.repo` — these files tell DNF that a repo is available at a certain location

**Example repo config fields (`/etc/yum.repos.d/appstream.repo`):**
- `name =` — human readable name
- `baseurl =` — location of the repo
- `enabled = 0/1` — whether DNF will check this repo or not
- `gpgcheck =` — should DNF verify package signatures before install

**BaseOS vs AppStream:**
- **BaseOS** — core OS, like kernel, utilities
- **AppStream** — user space applications and language runtimes

**Repo location types:**
- `file://` — if local
- `http://` — if remote

---

### RPM Commands

**Querying the database** (`rpm -q...`)
- RPM database location: `/var/lib/rpm`
- `rpm -q httpd` — check if a package is installed
- `rpm -qa` — show all installed packages
- `rpm -qa | wc -l` — count installed packages
- `rpm -qi package` — package info
- `rpm -ql package` — which files were installed
- `rpm -qR package` — what dependencies are needed

**Installing/managing with RPM:**
- `rpm -i package.rpm` — install
- `rpm -ivh package.rpm` — install with hash marks showing progress
- `rpm -Uvh package.rpm` — upgrade
- `rpm -e package` — remove

**If a package's signature is disabled/not verified:**
- Error appears saying the package isn't verified as genuine

---

### YUM
- YUM = "Yellowdog Updater Modified"
- Slower with large repos, slower dependency resolution, older resolution logic
- DNF = faster, newer nature, better dependency solver

**Common yum/dnf subcommands:**
- `install`
- `search`
- `info`
- `remove` / `erase`
- `update` — updates packages, doesn't remove obsolete packages and installs replacements

---

### `grep`
- Searches one or more text files for lines that match a specified pattern (searches inside files)
- Matches complete lines that contain the pattern
- Cannot grep binary files like `.png`, `.mp4`, `.mp3`
- Syntax: `grep [options] pattern file`
- Case-sensitive by default; use `-i` to ignore case
- `-n` — show line numbers along with matched output
- `-v` — show everything that does **not** match the pattern
- `-c` — count of matching lines
- `-r` — recursive search (search inside directories)

---

### `find`
- Searches for files/directories
- `find / -name "*.log"` — search for files by name pattern, starting from a given path

**Other find options:**
- `find / -type f` — search only files
- `find / -type d` — search only directories
- `-size +100M` / `-size -50M` — filter by size (larger/smaller than a given size)
- `-user` — filter by owner
- `-perm` — filter by permissions

**Running a command on found files:**
- `find / -name pattern -exec command {} \;` — runs a command on each file found

**Last modified time:**
- `-mtime -7` — modified less than 7 days ago
- `-mtime +30` — modified more than 30 days ago

---

### `xargs`
- Takes standard input and converts it into command line arguments
- Passes output as the next command's arguments
- Example: `find a.log b.log c.log | xargs rm`

---

### `sed` (Stream Editor)
- Used to edit text automatically, without opening a file in an editor
- `sed 's/old/new/' file` — to change all occurrences
  - `s` = substitute
  - `g` = global (replace all occurrences, not just the first)
- `sed -i` — to actually edit/modify the file (without `-i`, it just prints modified output on terminal, doesn't modify the original file)

**Deleting lines with sed:**
- `sed '3d' file` — delete line 3
- `sed '2,4d' file` — delete lines 2 to 4
- `sed '/^$/d' file` — delete blank lines

---
**Takeaway:** Covered package management in depth today — how RPM and DNF work together, repository structure and configuration, and key RPM/DNF query commands. Also covered powerful text-processing and file-searching tools: `grep` for pattern searching, `find` for locating files, `xargs` for converting input into arguments, and `sed` for automated text editing. These are essential day-to-day tools for a DevOps/Linux admin role.
