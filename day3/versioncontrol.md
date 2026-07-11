# Day 3 — Version Control with Git & GitHub

## What I learned today:

### What is Version Control & Why is it Needed?
- Version Control = maintaining versions of our application (upgrade/downgrade capability)
- If some problem occurs, we can go back to a previous stable version
- A critical component in collaborative software development
- Agile teams face frequent versioning-related problems
- Improves communication, code quality, provides collaboration tools, and version recovery
- Tracks what code changed and when

### Version & VCS — Basic Definitions
- Version = a kind of snapshot of the application at a point in time
- VCS (Version Control System) = a tool that manages, monitors, and tracks versions of code/application, and enables collaboration
- Without VCS: multiple developers working together leads to code overwrite, no proper backup maintained, and it's hard to trace which change introduced a bug
- If multiple developers work on the same project → need a way to backup, maintain, and track "who changed what"

---

### Types of Version Control Systems

**1. Local Version Control System**
- Single place (my computer, offline) to manage and review text/code
- Limitations: collaboration is difficult, scalability is limited, no single point of truth

**2. Centralized Version Control System (e.g., CVS/SVN)**
- Uses a client-server model
- Single central repository; all history stored on one central server
- Requires network availability to work; collaboration is possible

**3. Distributed Version Control System (e.g., Git)** — most powerful
- Created by Linus Torvalds
- Peer to peer — every developer has a local repo with full history and a complete backup
- Clone repo, push updates, resolve conflicts
- Multiple developers can work on the same repo independently, each with full access to complete history

---

### Git Basics
- Git — open source, distributed version control system
- Repo (Repository) — project/storage area where Git tracks the full history of files
- **Local repo** — on my own computer
- **Remote repo** — hosted on the internet (e.g., GitHub)
- Repository = a collection of files/folders that also tracks changes made to those files (a new "version" gets created whenever something is modified)
- `git init` — command to create a local repo (initializes a blank/untracked repo in the current system/folder)
- To work with Git locally: track the files/folders that need to be part of the repo, then initialize the repo using `git init`

---

### Git Workflow (Overview)
Working Directory - git add - Staging Area - git commit - Local Repo - git push - Remote Repo

- `git status` — tells the tracked, untracked, and modified status of files

---

### Staging & Tracking Files
- `git add filename` — add a specific file to the staging area (starts tracking it)
- `git add .` — add all changed files to the staging area at once
- Once a file is staged, if you edit it again and re-add, only the new changes get staged (not tracked again from scratch)
- `git diff` — shows the difference between staged and unstaged (working directory) changes

### Committing
- `git commit -m "message"` — saves the staged files into the local repo as a permanent snapshot, with a message describing the change
- Only staged files get committed

### Pushing
- `git push` — pushes committed files from the local repo to the remote repo (GitHub)
- Only committed files are pushed (not just staged/untracked ones)

### Undoing / Unstaging
- `git restore filename` — go back/discard changes in a file that hasn't been staged yet
- `git restore --staged filename` — unstage a file (moves it back from staging area to working directory, keeps the changes)
- `git rm --cached filename` — used to unstage/untrack a file

---

### Git Configuration
- `git config --global user.name "your name"` — sets committer name
- `git config --global user.email "your email"` — sets committer email
- **Global** — user-specific setting; the currently logged-in user can access it, applies across all repos
- **Local** — folder-wise/particular-repo setting; overrides global for that specific repo
- **System** — applies to all users on the whole system
- **Override priority:** local > global > system
- `git config --list` — check current configuration
- `git config --global --unset user.name` — remove a global config value
- git config --global --unset user.email` — remove a global config value
  (after unset delink credentials)
- `git config --global core.editor "code"` — sets VS Code as the default editor for commit messages
- `git config --global color.ui auto` — enables color-coded output for status/diff
- Alias example: `alias.co checkout` → lets you type `git co` instead of `git checkout`

---

### Branching
- When a repo is initialized, Git provides a default branch — `master` (older convention) or `main` (newer convention)
- `git branch` — lists branches, shows current branch with `*`
- `git branch -M main` — renames the current branch (e.g., master → main)
- Creating a new branch: `git branch branchname`
- Creating and switching to a new branch in one step: `git switch -c branchname` or `git checkout -b branchname`
- Just switching to an existing branch (no create, no `-c`): `git switch branchname` or `git checkout branchname`
- Branches are used to add a new feature or fix a bug, then merge back into the existing/main code

### Merging
- `git merge branchname` — merges the specified branch into the current branch
- After merging, the now-unneeded branch can be deleted: `git branch -d branchname`

### Merge Conflicts
- Happens when the same file/lines are edited differently in two branches, and Git can't automatically decide which version to keep
- Example: if the main branch has changes to a file, and another branch also edited and committed changes to the same file, merging causes a conflict
- To resolve a merge conflict,
  Resolve conflict manually in the file - stage - commit - push

  ---

### Connecting to a Remote Repository (GitHub)
- `git remote add origin <url>` — connects local repo to a remote GitHub repo
- `git remote -v` — verify the connected remote URL(s)
- **How to reset/change the origin:** `git remote set-url origin <new-repo-url>`
- `git push -u origin main` — pushes local branch to remote and sets it as the upstream/default branch for future pushes (only needed once — after that, plain `git push` works)
- Note: sometimes need to delink the credential manager (Windows Credential Manager) if remote/auth gets stuck

**Two ways to connect a local project to GitHub:**
1. Open an existing repo folder locally → connect it to a new GitHub repo using the `git remote add origin` command
2. Create a new repo on GitHub (choose public/private) → copy the setup commands GitHub provides → paste and run them locally, then `git push origin main` to set the default branch

- `git clone <url>` — download/copy an existing remote repo to the local system
- `git pull` — pull the latest changes from the remote repo into local

---

### Log, Diff & Show
- `git log` — shows commit history
- `git log --oneline` — shows commit history in a compact, one-line-per-commit format (useful to see the latest/last commit quickly)
- `git diff` — shows changes compared (between working directory and staging, or between commits)
- `git show` — shows detailed info about a specific commit: commit ID, author, message, and the exact changes made

---

### GitHub Repo Setup Files
- **README** — the first thing people see when they open a repo; acts like a welcome/greeting card for the project
- **LICENSE** — tells others how they're allowed to use the code
- **.gitignore** — a file listing which files/folders Git should ignore (not track/push to GitHub) — e.g., temporary files, credentials, config files that shouldn't be shared

---

  
