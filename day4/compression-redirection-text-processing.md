# Day 04 — Compression, Archiving, Redirection & Text Processing

## What I learned today:

### Compression vs Archiving — Basics
- **Compression** — reduces the size of a file
- **Archive** — combines multiple files/folders into one file (doesn't necessarily reduce size)
- Some tools do both — compress and archive successfully

### Compression Tools

**1. gzip** (most common)
- `gzip file` → creates `file.gz` (original file replaced by default)
- `gzip -c file > file.gz` — keeps both original and compressed files available (redirects output instead of replacing)
- **Compression level** — how aggressively data gets compressed
  - `gzip -1` — fastest speed, lowest compression (minimum)
  - `gzip -6` — default level
  - `gzip -9` — highest compression, slowest speed (maximum)
- `gunzip file.tar.gz` or `gzip -d file.gz` — decompress
- `gzip -t file` — test file integrity
- `gzip -l` — list info (original size, compressed size, ratio)

**2. bzip2**
- `bzip2 file.txt` → creates `file.txt.bz2`
- `bunzip2 file.txt.bz2` — decompress

**3. xz**
- Best compression among built-in tools
- `xz file.txt` → creates `file.txt.xz`
- `unxz` — decompress

**4. zip**
- Archives and compresses together (unlike gzip which only compresses one file)
- `zip file.zip file1 file2 file3` — creates a zip archive with multiple files
- `unzip file.zip` — extract

---

### tar (Tape Archive)
- Used to combine (archive) multiple files/folders into one file
- `tar -cvf backup.tar folder/` — create an archive
  - `c` = create, `v` = verbose (show progress), `f` = filename

**Combining tar with compression:**
- `tar -czvf backup.tar.gz folder/` — tar + gzip compression
- `tar -cjvf backup.tar.bz2 folder/` — tar + bzip2 compression
- `tar -cJvf backup.tar.xz folder/` — tar + xz compression

**Extracting:**
- `tar -xvf backup.tar` — extract a tar file
- `tar -xzvf backup.tar.gz` — extract a gzip-compressed tar file
- `tar -xjvf backup.tar.bz2` — extract a bzip2-compressed tar file
- `tar -xJvf backup.tar.xz` — extract an xz-compressed tar file

**Other useful tar options:**
- `tar -tf backup.tar` — list contents without extracting
- `-p` — preserve permissions
- `--exclude` — skip specific files while archiving
- `--remove-files` — delete source files after archiving
- `--append` — append files to an existing uncompressed archive
- `--delete` — delete files from an uncompressed archive

---

### Redirection
- Every Linux command has 3 standard streams (file descriptors), identified by non-negative integers used by the OS:
  - **stdin (0)** — standard input, provided from keyboard
  - **stdout (1)** — standard output, normal successful output
  - **stderr (2)** — error messages
- stdout and stderr both appear on the terminal but are technically different streams

**Output redirection:**
- `>` — overwrite (send output to a file, replacing existing content)
- `>>` — append (add output to the end of a file without erasing existing content)
- Example: `ls file > output.txt` — sends output to a file
- Example: `ls file 2> error.txt` — sends only errors to a file
- Example: `ls file > file.txt 2>&1` — sends both output and errors to the same file

**Input redirection:**
- `<` — takes input from a file instead of keyboard
- Example: `wc < names.txt` — counts words using the file as input

**Discard output:**
- `> /dev/null` — used to discard/ignore output entirely

**Error redirection specifically:**
- `2>` — redirect only error output to a file

---

### `tee` Command
- Sends stdout to two places at once — to a file AND to the terminal screen
- `command | tee file.txt` — write output to file, and also print output on terminal
- `command | tee -a file.txt` — append instead of overwrite

---

### Pipes (`|`)
- Connects the stdout of one command to the stdin of another
- Example: `cat file.txt | grep new` — searches for the word "new" in the file
- **Exit status** — refers to the last command in a pipeline

---

### Text Processing Commands in Linux
1. **cat** — display file contents
2. **tac** — reverse of cat, displays content in reverse order
3. **head** — shows top lines (default: first 10)
   - `head -5 file` — show first 5 lines
4. **tail** — shows last lines (default: last 10)
   - `tail -5 file` — show last 5 lines
5. **less** — view file page by page
   - `space` — next page
   - `b` — previous page
   - `/word` — search for a word
   - supports up/down scroll
6. **more** — older version of `less`, simpler pager
7. **wc** — word count; shows lines, words, and bytes (e.g., `10 50 350`)
8. **sort** — sort lines in alphabetical order
9. **uniq** — remove duplicate (adjacent) lines
10. **cut** — extract specific columns
    - `cut -d "," -f1 file` — cut using comma as delimiter, extract field 1
11. **tr** — convert characters (e.g., uppercase to lowercase)
12. **grep** — global regular expression print (search for patterns in text)
13. **find** — search for files

---

### Wildcards
- `.` — matches any character
- `^` — start of a line
- `$` — end of a line
- `*` — zero or more occurrences
- `+` — one or more occurrences
- `?` — exactly one occurrence
- `{}` — brace expansion

---
**Takeaway:** Covered file compression and archiving (gzip, bzip2, xz, zip, tar), stream redirection (stdout/stderr/stdin), pipes for chaining commands, and essential text-processing commands 
