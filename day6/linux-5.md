# Day 06 — Process Monitoring, Signals & Shell Basics

## What I learned today:

### Controlling Commands (Kill)
- `kill PID` — kill a process by its PID
- `kill nginx` — needs the exact PID, kills all processes having the same name
  - A name (like `nginx`) can have several processes running, not just one PID
- `kill -15 PID` — graceful kill; lets the process complete its work then exit
- `kill -9 PID` — forceful kill; terminates immediately
- `pkill` — kill by name, pattern, or user (more flexible)
  - Example: `pkill -u harshita` — kills all processes under that user

### `top` — Real-time Process Monitoring
- Columns: `PID`, `USER`, `PR`, `NI`, `VIRT`, `RES`, `SHR`, `S`, `%CPU`, `%MEM`, `TIME+`, `CMD`
  - `PR` — priority, `NI` — nice value
  - `VIRT` — virtual memory allocated, `RES` — resident memory, `SHR` — shared memory
  - `S` — state, `TIME+` — CPU time used

**Interactive `top` options:**
- `M` — sort by RAM/memory usage
- `P` — sort by CPU usage
- `d` — change refresh rate
- `k` — kill a process, then enter PID interactively

---

### Nice & Renice — Process Priority
- **Priority** — more priority means more chances to get CPU time
- **Nice** — how "nice" a process is to others (inverse relation to priority)
  - Example: one polite/nice student lets others go in front instead of pushing ahead — while a "not nice" person doesn't let others go, gets more CPU
- **Nice value range:** -20 to +19
  - **-20** — highest priority
  - **0** — default
  - **+19** — lowest priority
- `nice -n value command` — used while starting a process, helps the CPU scheduler decide which process to prioritize
  - Lower nice value → will receive more CPU time
- `renice` — change/modify the already-set priority of a running process
  - Syntax: `renice NICE_VALUE -p PID`

---

### `sleep` and `wait`
- `sleep` — pauses the current process for a specified time (e.g., `sleep 2m`); doesn't use CPU while paused
- `wait` — pauses the shell until a background job is finished

---

### Signals
- A signal is an interrupt sent to a process
- **Why needed** — if a process is running, this is how we tell it to pause, stop, exit, etc.
- **Signals sent by:** user, keyboard (`Ctrl+C`), kernel, another process

**Signal flow example:**
- Example: `sleep 10` is running, then I press `Ctrl+C`
- Shell receives keyboard input → sends request to kernel (asks kernel) → kernel sends the signal → the sleep process receives the signal → sleep terminates

**Common Signals (to list all: `kill -l`):**
- `SIGTERM` — terminate (graceful)
- `SIGKILL (kill -9)` — force kill immediately
- `SIGSTOP` — pause
- `SIGCONT` — continue/resume
- `SIGINT` — interrupt (Ctrl+C)
- Signals are **asynchronous** — can arrive at any time

**Signal Handler**
- A function that runs/executes when a process receives a specific signal

**`trap`**
- Used to tell a shell script what to do when a specific signal arrives

---

### Shell Initialization Files
- A script containing shell commands that runs automatically

**Shell types:**
- **Login shell** — starts after logging in (authentication involved)
- **Non-login shell** — already logged in, e.g., opening a new terminal window

**Interactive vs Non-interactive:**
- **Interactive** — gives a prompt after execution, waits for another command
- **Non-interactive** — e.g., running a script — no prompt, no waiting for the next command

**Config files:**
- `/etc/profile` — system-wide login config, executed by every user's login shell
- `/etc/bashrc` — settings for interactive bash shells (aliases, etc.)
- `~/.bash_profile` — user's own login shell config file
- `~/.bashrc` — configures every interactive shell for that user

---

### Alias
- Shortcut for a command
- To view aliases: `alias`
  - Example: `alias ll='ls -la'`
- To make permanent: add it in `~/.bashrc`
- To remove: `unalias -a` — removes all current shell aliases (but not permanently, unless also removed from `~/.bashrc`)

---

### `history`
- Command to view shell history
- Stored in `~/.bash_history`

---
**Takeaway:** Covered process monitoring and control today — `ps`/`top` command variations, killing processes (`kill`, `pkill`), process priority with `nice`/`renice`, `sleep`/`wait`, signals and how they flow from keyboard to kernel to process, plus shell fundamentals — login vs non-login shells, shell initialization files, aliases, and command history. Strong toolkit for managing and troubleshooting running processes in Linux.
