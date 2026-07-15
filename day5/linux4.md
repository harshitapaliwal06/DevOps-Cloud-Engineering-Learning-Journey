# Day 05 — Process Management

## What I learned today:

### What is a Process?
- running instance of program
- program whoch is currently executed 

### Process Management (Overview)
- Kernel creates a process
- Scheduler picks the process (uses CPU scheduler)
- CPU executes the process instructions
- Once you execute a program, it is loaded into memory → CPU
- Steps: program written -> stored on disk -> loaded into RAM -> running (process)

### Program vs Process
- A program is a set of instructions (executable file stored on disk)
- Once you execute it, it becomes a process — loaded in memory and running

###  what happens when we run a command (eg ls)
keyboard send ls to shell -> shell searches for executable -> kernel loads executable -> kernel assigns resources (like PID) -> cpu executes instructions -> process finished , memory is freed , pid removed 

---

### Stages of a Process (Process Lifecycle)
- **Ready to execute** — process created, ready to run
- **Running** — CPU executes the process's instructions
- **Waiting** — process waiting for I/O or some resource; runs again after getting the resource
- **Terminated/Finished** — process execution is complete

---

### `fork()` and `exec()`
- `fork()` — makes a copy of itself (creates a new process)
  - Example: opening a terminal → bash is running → if I run `ls`, bash creates a child process using fork
  - Result: Parent bash → Child bash
- `exec()` — replaces the current process image with a new program
  - Example: `exec(ls)` → Parent bash → Child becomes `ls` (child process is replaced by the `ls` program)
- **Summary:**
  - `fork()` — creates a new process
  - `exec()` — replaces/loads a new program into the current process

---

### Process States (Status Codes)
- **R** = Running — currently using the CPU
- **S** = Interruptible Sleep — waiting; can wake up when needed (e.g., waiting for a resource)
- **D** = Uninterruptible Sleep — usually waiting on I/O/disk; cannot wake up, ignores signals (even `kill -9` is not useful here)
- **T** = Stopped
- **Z** = Zombie — process has finished, but its entry hasn't been removed from the process table yet

---

### PID and PPID
- **PID (Process ID)** — a unique ID allocated to every process (used to identify each process)
- Every process, except PID 1, has a parent process
- **PPID (Parent Process ID)** — the PID of the process that created the current process
  - The process that creates other processes is the parent; the created ones are child processes
  - One parent can have many children
  - Example: Bash (PPID) → fork → creates children like `whoami`, `pwd`, `ls`, `exec`
  - Children can also become parents themselves

- Linux processes form a **process tree/family**
- **PID 1** — parent process of all processes on the system — this is `systemd`
  - `systemd` starts all other services (it's the "manager")
  - PPID of `systemd` = 0, because the kernel started it directly
- If a parent process dies, its child becomes an **orphan process**
- PID 1 (systemd) adopts orphan processes (i.e., parent exited but the child is still running)

---

### Foreground and Background Processes
- **Foreground process** — occupies the terminal and receives input directly from the user
  - Cannot use the terminal for anything else until the process is stopped, finished, or moved to background
  - Example: `sleep 20` — while running, terminal is unusable until it finishes
- **Background process** — runs in the background, does not occupy the terminal
  - Example: `sleep 60 &` — when run with `&`, it returns a prompt immediately and runs in background
  - **Job number** — shown in brackets when a job is sent to background, e.g., `[1] 5432` (5432 is the PID)

---

### Job Control Commands
- `&` — starts a process in the background directly
- Job given by shell → shows jobs command to see all background jobs
- `Ctrl + Z` — stops (pauses) the current foreground process, moves it to a "stopped" state
- `bg` — resumes a stopped job and runs it in the background
- `Ctrl + C` — terminates the current process
- To send a background job to the foreground: `fg %1` — the process comes back into the foreground

### `nohup`
- Used so that a process keeps running even after the terminal is closed
- "No hangup" — the process is detached from the terminal session
- Example: `nohup python.py &`

### Removing a Job
- `disown %1` — removes a job from the job table
  - The process still runs, but its entry is removed and it can no longer be brought to the foreground

### Daemons
- Run in the background
- Usually start automatically during boot
- Provide services
- User does not interact with them directly
- Stop only when explicitly stopped or on shutdown

---

### Process Monitoring Commands

**`ps` (process status)** — displays info about processes
- Columns: `PID`, `TTY`, `TIME`, `CMD`
  - `TTY` — terminal associated with the process
  - `TIME` — CPU time used by the process
  - `CMD` — command/program name
- By default, `ps` only shows processes associated with the current terminal
- `ps -e` — shows all/every process in the system
-  `ps -f` — shows output in full format
- `UID` (userid) , ` stime` (starttime) , `C` (schedular value) ,`PPID`(parent process id)  these fields are added in ouput 
- `ps -ef` - v-style full style info in full format 

**`ps aux`** — BSD-style output, shows a wider process view
- Columns: `USER`, `PID`, `%CPU`, `%MEM`, `VSZ`, `RSS`, `TTY`, `STAT`, `START`, `TIME`, `CMD`
  - `%CPU` — CPU usage
  - `%MEM` — memory usage
  - `VSZ` — virtual memory size
  - `RSS` — resident (physical) memory used
  - `STAT` — process state
  - `START` — start time
- `ps -ef` — similar full-format process listing, System V style

**Other `ps` variations:**
- `ps -o` — custom output format, e.g., `ps -o pid,cmd,tty` — show only selected columns
- `pgrep nginx` — outputs the PID of a process by name (e.g., nginx)
- `pidof nginx` — gives the PID of nginx

**`pstree`** — shows the process tree (parent-child relationships) visually

---

### Controlling Commands
- Covers signals and controls used to manage running processes (stop, kill, resume, etc.)

---
**Takeaway:** Covered process management fundamentals today — the process lifecycle, fork/exec, process states, PID/PPID and the process tree, foreground vs background jobs, daemons, and key process monitoring commands (`ps`, `ps aux`, `pgrep`, `pidof`, `pstree`). Essential for troubleshooting and managing systems in a DevOps role.
