# Linux Commands Every DevOps Engineer Must Know
## Complete Reference with Purpose, Importance, and Real-World Usage

> **Who this is for:** DevOps Engineers, Cloud Engineers, SREs, Platform Engineers
> **Why Linux matters:** 90%+ of servers, containers, and cloud VMs run Linux. Every kubectl exec, docker exec, SSH session, and CI runner drops you into a Linux shell. These commands are your daily tools.

---

## How to Read This Document

Each command entry answers four questions:
- **What it does** — the precise definition
- **Why DevOps engineers need it** — the real-world reason it matters
- **Core syntax** — the flags you will actually use
- **DevOps scenario** — a real situation where this saves you

---

## Table of Contents

- [Category 1 — Navigation and Directory Operations](#category-1--navigation-and-directory-operations)
- [Category 2 — File Operations](#category-2--file-operations)
- [Category 3 — Viewing and Reading Files](#category-3--viewing-and-reading-files)
- [Category 4 — Text Processing — The Power Tools](#category-4--text-processing--the-power-tools)
- [Category 5 — File Permissions and Ownership](#category-5--file-permissions-and-ownership)
- [Category 6 — Process Management](#category-6--process-management)
- [Category 7 — System Monitoring and Performance](#category-7--system-monitoring-and-performance)
- [Category 8 — Disk and Storage](#category-8--disk-and-storage)
- [Category 9 — Networking](#category-9--networking)
- [Category 10 — Archive and Compression](#category-10--archive-and-compression)
- [Category 11 — User and Permission Management](#category-11--user-and-permission-management)
- [Category 12 — Package Management](#category-12--package-management)
- [Category 13 — Services and Systemd](#category-13--services-and-systemd)
- [Category 14 — SSH and Remote Operations](#category-14--ssh-and-remote-operations)
- [Category 15 — Shell and Scripting Essentials](#category-15--shell-and-scripting-essentials)
- [Category 16 — Git for DevOps](#category-16--git-for-devops)
- [Category 17 — Docker CLI](#category-17--docker-cli)
- [Category 18 — Kubernetes — kubectl](#category-18--kubernetes--kubectl)
- [Category 19 — Security and Audit](#category-19--security-and-audit)
- [Category 20 — DevOps One-Liners and Power Combos](#category-20--devops-one-liners-and-power-combos)

---

---

## Category 1 — Navigation and Directory Operations

---

### `pwd` — Print Working Directory

**What it does:** Prints the absolute path of your current directory.

**Why DevOps engineers need it:** When you SSH into a server or exec into a container, you land somewhere. Before running any destructive command (`rm -rf`, `chmod -R`) you must know exactly where you are. One wrong `rm -rf ./` from the wrong directory has deleted production data.

**Syntax:**
```bash
pwd
# Output: /var/log/nginx
```

**DevOps scenario:** You've exec'd into a Kubernetes pod to debug. Before deleting temp files, confirm you're in `/tmp` not `/`.

---

### `ls` — List Directory Contents

**What it does:** Lists files and directories in the current or specified directory.

**Why DevOps engineers need it:** You look at directory contents dozens of times per day — checking config files, log directories, deployment artifacts, Docker volumes.

**Syntax:**
```bash
ls                        # Basic list
ls -l                     # Long format (permissions, size, date, owner)
ls -la                    # Long format including hidden files (dotfiles)
ls -lh                    # Human-readable file sizes (KB, MB, GB)
ls -lt                    # Sort by modification time (newest first)
ls -lS                    # Sort by size (largest first)
ls -lR                    # Recursive (show subdirectories)
ls -ld /etc/nginx         # Show directory itself, not its contents

# Real-world combination
ls -lah /var/log/         # All files, human-readable sizes, hidden included
```

**DevOps scenario:** A disk is filling up. `ls -lhS /var/log/` shows the largest log files sorted by size so you know what to rotate or delete first.

---

### `cd` — Change Directory

**What it does:** Changes your current working directory.

**Why DevOps engineers need it:** Navigation is constant. You move between config dirs, log dirs, application dirs, and home directories constantly.

**Syntax:**
```bash
cd /var/log/nginx         # Absolute path
cd ..                     # Go up one level
cd ../..                  # Go up two levels
cd ~                      # Go to home directory
cd -                      # Go back to previous directory (toggle)
cd /                      # Go to root

# The most useful trick — cd with previous directory
cd /very/long/path/to/config
cd /another/long/path
cd -                      # Returns to /very/long/path/to/config
```

**DevOps scenario:** `cd -` is a lifesaver when switching between two directories repeatedly — config dir and log dir during debugging.

---

### `mkdir` — Make Directory

**What it does:** Creates directories.

**Why DevOps engineers need it:** Creating directory structures for applications, logs, configs, and deployment artifacts.

**Syntax:**
```bash
mkdir logs                          # Create single directory
mkdir -p /opt/myapp/logs/archive    # Create full path (no error if exists)
mkdir -p /app/{config,logs,data}    # Create multiple directories at once
mkdir -m 755 /opt/myapp             # Create with specific permissions
```

**DevOps scenario:** A deployment script creates the required directory structure before the app starts: `mkdir -p /opt/myapp/{config,logs,data,tmp}`

---

### `find` — Search for Files and Directories

**What it does:** Searches for files and directories based on name, type, size, age, permissions, and more. Then optionally acts on them.

**Why DevOps engineers need it:** The most powerful file-finding tool in Linux. Used for cleanup jobs, security audits, config discovery, and log management. Every DevOps engineer uses `find` daily.

**Syntax:**
```bash
# Find by name
find /etc -name "nginx.conf"               # Find nginx.conf anywhere in /etc
find /var/log -name "*.log"                # Find all .log files
find . -name "*.py" -type f               # Only regular files, not directories
find / -name "*.key" 2>/dev/null          # Find keys, suppress permission errors

# Find by time
find /var/log -mtime +7                    # Modified more than 7 days ago
find /var/log -mtime -1                    # Modified in the last 24 hours
find /tmp -atime +30                       # Accessed more than 30 days ago

# Find by size
find / -size +100M                         # Files larger than 100MB
find / -size +1G -type f                  # Files larger than 1GB

# Find by permissions (security audits)
find / -perm -4000 -type f               # SUID files (security risk!)
find / -perm -o+w -type f               # World-writable files

# Find and execute action
find /var/log -name "*.gz" -mtime +30 -delete          # Delete old compressed logs
find /tmp -name "*.tmp" -exec rm {} \;                  # Delete all .tmp files
find . -name "*.conf" -exec chmod 640 {} \;             # Fix permissions on all configs

# Find by owner
find /home -user alice -type f                          # All files owned by alice
find /var/www -not -user www-data                       # Files NOT owned by www-data
```

**DevOps scenario:** Disk alert at 90%. `find / -size +500M -type f -ls 2>/dev/null | sort -k7 -rn | head -20` finds the 20 largest files across the entire filesystem.

**DevOps scenario 2:** Security audit — `find / -perm -4000 -type f 2>/dev/null` lists all SUID binaries which can be privilege escalation vectors.

---

---

## Category 2 — File Operations

---

### `cp` — Copy Files and Directories

**What it does:** Copies files or directories.

**Why DevOps engineers need it:** Copying config files before editing (backup), deploying files, creating backups.

**Syntax:**
```bash
cp source.txt dest.txt               # Copy file
cp -r source_dir/ dest_dir/         # Copy directory recursively
cp -a source/ dest/                  # Archive mode: preserve permissions, timestamps, ownership
cp -p source.conf dest.conf         # Preserve file attributes
cp -i source.txt dest.txt           # Prompt before overwriting (interactive)
cp -u source.txt dest.txt           # Only copy if source is newer

# Always back up before editing
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak.$(date +%Y%m%d)
```

**DevOps scenario:** Before every config edit: `cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup`. If nginx reload fails, `cp /etc/nginx/nginx.conf.backup /etc/nginx/nginx.conf` and you're back to working state.

---

### `mv` — Move / Rename Files

**What it does:** Moves or renames files and directories.

**Why DevOps engineers need it:** Deploying new config versions, rotating logs, renaming files atomically.

**Syntax:**
```bash
mv old_name.txt new_name.txt         # Rename
mv file.txt /tmp/file.txt            # Move to different location
mv -i source dest                    # Prompt before overwriting
mv config.conf.new config.conf       # Atomic config replacement
```

**DevOps scenario:** Atomic config deployment. Write new config to `config.conf.new`, validate it, then `mv config.conf.new config.conf` — the move is atomic so there's no moment where a partially-written config exists.

---

### `rm` — Remove Files

**What it does:** Deletes files and directories permanently (no recycle bin).

**Why DevOps engineers need it:** Cleaning up logs, temp files, old deployments. Also the most dangerous command — used wrong it deletes everything.

**Syntax:**
```bash
rm file.txt                   # Delete a file
rm -f file.txt                # Force delete (no error if missing)
rm -r directory/              # Delete directory recursively
rm -rf directory/             # Force recursive delete (DANGEROUS)
rm -i *.log                  # Interactive — prompt for each file

# Safe practices
rm -rf /tmp/build_artifacts/  # OK — specific path
# NEVER: rm -rf /            # Deletes entire filesystem!
# NEVER: rm -rf ~            # Deletes home directory!
# NEVER: rm -rf ./ *         # Dangerous space issue
```

**DevOps scenario:** Cleanup script after CI build: `rm -rf "${BUILD_DIR}/artifacts/"` — always use a variable with a trailing path, never just `rm -rf ${BUILD_DIR}` (if BUILD_DIR is empty, this deletes current directory).

**The golden rule:** Always quote variables in rm commands. `rm -rf "$TMPDIR/"` is safe. `rm -rf $TMPDIR/` can be catastrophic if TMPDIR is unset.

---

### `ln` — Create Links

**What it does:** Creates hard links and symbolic (soft) links between files.

**Why DevOps engineers need it:** Symlinks are used everywhere in Linux — nginx/apache sites-enabled, systemd service files, Python virtual environments, config management.

**Syntax:**
```bash
ln -s /path/to/real/file /path/to/symlink     # Create symbolic link
ln -sf /path/to/new /path/to/symlink          # Force (replace existing symlink)
ln /etc/real-config.conf /etc/config.conf     # Hard link (same inode)

# Common DevOps patterns
ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/mysite
ln -s /opt/myapp/current/bin/myapp /usr/local/bin/myapp
```

**DevOps scenario:** Deploying a new application version. `ln -sf /opt/myapp/v2.1.0 /opt/myapp/current` changes the symlink atomically — all processes reading `/opt/myapp/current` immediately see the new version.

---

---

## Category 3 — Viewing and Reading Files

---

### `cat` — Concatenate and Print Files

**What it does:** Prints file contents to stdout. Also concatenates multiple files.

**Why DevOps engineers need it:** Quick inspection of config files, log files, scripts. Piping content to other commands.

**Syntax:**
```bash
cat /etc/nginx/nginx.conf            # Print file
cat -n file.txt                      # Print with line numbers
cat file1.txt file2.txt > combined  # Concatenate files
cat /dev/null > file.txt            # Empty a file without deleting it

# Check environment file
cat /etc/environment
cat /proc/cpuinfo                    # CPU information
cat /proc/meminfo                    # Memory information
```

**DevOps scenario:** `cat /proc/1/environ | tr '\0' '\n'` shows the environment variables of PID 1 — useful inside containers to verify env vars were injected correctly.

---

### `less` — Page Through Files

**What it does:** Opens a file for scrolling (forward and backward). Unlike `cat`, it doesn't load the entire file into memory.

**Why DevOps engineers need it:** Large log files (access.log can be GBs). You never `cat` a multi-GB file — it floods the terminal and uses enormous memory.

**Syntax:**
```bash
less /var/log/nginx/access.log

# Navigation inside less:
# Space / f   — scroll forward one page
# b           — scroll back one page
# G           — jump to end of file
# g           — jump to beginning
# /pattern    — search forward
# ?pattern    — search backward
# n           — next search result
# N           — previous search result
# q           — quit

less +G /var/log/app.log     # Open at the end (like tail)
less +F /var/log/app.log     # Follow mode (like tail -f, press Ctrl+C to stop)
```

**DevOps scenario:** Investigating a production incident. `less +G /var/log/nginx/error.log` opens at the end of the log, then `/error` finds all error messages.

---

### `tail` — View End of File

**What it does:** Shows the last N lines of a file. With `-f`, follows the file as new lines are written.

**Why DevOps engineers need it:** `tail -f` is the most-used log monitoring command in production. Every DevOps engineer has multiple terminal tabs with `tail -f` running.

**Syntax:**
```bash
tail /var/log/syslog              # Last 10 lines (default)
tail -n 50 /var/log/nginx/error.log   # Last 50 lines
tail -f /var/log/app/app.log     # Follow in real time (Ctrl+C to stop)
tail -F /var/log/app/app.log     # Follow even if file is rotated/recreated

# Follow multiple files simultaneously
tail -f /var/log/nginx/access.log /var/log/nginx/error.log

# Follow and filter
tail -f /var/log/app.log | grep ERROR
tail -f /var/log/app.log | grep -v "health-check"   # Exclude health check noise
```

**DevOps scenario:** Deploying a new version. In one terminal: `kubectl rollout status deployment/api`. In another: `kubectl logs -f deployment/api | grep -E "ERROR|WARN|started"`. Watch deployment progress and any errors in real time.

---

### `head` — View Beginning of File

**What it does:** Shows the first N lines of a file.

**Why DevOps engineers need it:** Check file headers, CSV column names, config file structure without loading the whole file.

**Syntax:**
```bash
head /etc/passwd              # First 10 lines (default)
head -n 20 access.log         # First 20 lines
head -n 1 data.csv            # Just the header row of a CSV
head -c 100 binary_file       # First 100 bytes
```

**DevOps scenario:** `head -1 /var/log/app.log` shows the log format so you know what fields to parse with `awk` before processing thousands of lines.

---

### `grep` — Search Text Patterns

**What it does:** Searches files (or stdin) for lines matching a pattern (plain text or regex). Returns matching lines.

**Why DevOps engineers need it:** Log analysis is 50% of production debugging. `grep` is the fastest way to find errors, IPs, request IDs, and patterns in logs. Every DevOps engineer uses grep hundreds of times per day.

**Syntax:**
```bash
grep "ERROR" /var/log/app.log                     # Find lines with ERROR
grep -i "error" /var/log/app.log                  # Case-insensitive
grep -n "CRITICAL" /var/log/app.log               # Show line numbers
grep -v "DEBUG" /var/log/app.log                  # Invert (lines WITHOUT DEBUG)
grep -c "ERROR" /var/log/app.log                  # Count matching lines
grep -l "ERROR" /var/log/*.log                    # List files containing ERROR
grep -r "password" /etc/                          # Recursive search in directory
grep -A 3 "EXCEPTION" app.log                     # Show 3 lines AFTER match
grep -B 3 "EXCEPTION" app.log                     # Show 3 lines BEFORE match
grep -C 5 "EXCEPTION" app.log                     # Show 5 lines AROUND match

# Extended regex (-E or egrep)
grep -E "ERROR|CRITICAL|FATAL" app.log            # Multiple patterns (OR)
grep -E "^2024-01-15.*ERROR" app.log              # Regex: date + ERROR on same line
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" access.log  # IP addresses

# Recursive with file type
grep -r --include="*.conf" "worker_processes" /etc/nginx/

# Pipeline usage (most common)
kubectl logs deployment/api | grep "ERROR"
cat /var/log/auth.log | grep "Failed password"
ps aux | grep nginx
```

**DevOps scenario:** Production alert — 500 errors spiking. `grep " 500 " /var/log/nginx/access.log | tail -100 | awk '{print $7}' | sort | uniq -c | sort -rn` — finds which URLs are returning 500s and how many times.

---

---

## Category 4 — Text Processing — The Power Tools

---

### `awk` — Pattern Scanning and Processing

**What it does:** Processes text line by line, splitting each line into fields. Can filter, transform, calculate, and format text output.

**Why DevOps engineers need it:** Log files are structured text. `awk` extracts specific columns, calculates statistics, and transforms output. Every time you need "give me column 4 of this output" — that's `awk`. Indispensable for log analysis, system monitoring, and automation scripts.

**Syntax:**
```bash
# Basic: $1 = first field, $2 = second, $NF = last field
awk '{print $1}' file.txt                    # Print first column
awk '{print $1, $4}' file.txt               # Print 1st and 4th columns
awk '{print NR, $0}' file.txt               # Print line numbers
awk 'NR==5' file.txt                         # Print only line 5
awk 'NR>=10 && NR<=20' file.txt             # Print lines 10-20

# With field separator (-F)
awk -F: '{print $1}' /etc/passwd             # Print usernames (: delimited)
awk -F',' '{print $2}' data.csv             # Print 2nd CSV column
awk -F'\t' '{print $3}' data.tsv            # Tab-separated

# Filtering + pattern matching
awk '/ERROR/ {print}' app.log               # Lines containing ERROR
awk '$9 == "200" {print $7}' access.log     # HTTP 200 URLs from nginx log
awk '$9 >= "500" {print $0}' access.log     # 5xx errors

# Calculations
awk '{sum += $5} END {print "Total:", sum}' data.txt     # Sum column 5
awk 'END {print NR}' file.txt               # Count lines (like wc -l)
awk '{print $NF}' file.txt                  # Print last field of each line

# Real DevOps power: nginx log analysis
# Format: IP - - [date] "METHOD URL HTTP" STATUS SIZE
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -rn
# → Count HTTP status codes

awk '$9 >= 400 {print $7}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -20
# → Top 20 URLs returning errors

awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -10
# → Top 10 client IP addresses by request count

# Memory usage from ps
ps aux | awk 'NR>1 {sum += $4} END {printf "Total RAM: %.1f%%\n", sum}'
```

**DevOps scenario:** Analysing a slow API. `awk '{print $NF}' /var/log/nginx/access.log | awk '{sum+=$1; count++} END {printf "Avg response: %.0fms\n", sum/count*1000}'` calculates average response time across millions of log entries in seconds.

---

### `sed` — Stream Editor

**What it does:** Performs find-and-replace, deletion, and insertion operations on text streams line by line.

**Why DevOps engineers need it:** Modifying config files in deployment scripts without opening an editor. Templating — replacing placeholders with actual values. Processing log output.

**Syntax:**
```bash
# Find and replace
sed 's/old/new/' file.txt                    # Replace first occurrence per line
sed 's/old/new/g' file.txt                  # Replace ALL occurrences
sed 's/old/new/gi' file.txt                 # Case-insensitive replace all
sed -i 's/old/new/g' file.txt               # In-place edit (modifies file!)
sed -i.bak 's/old/new/g' file.txt           # In-place with backup

# Delete lines
sed '/pattern/d' file.txt                   # Delete lines containing pattern
sed '/^#/d' config.conf                     # Delete comment lines
sed '/^$/d' file.txt                        # Delete empty lines
sed '5d' file.txt                           # Delete line 5
sed '5,10d' file.txt                        # Delete lines 5-10

# Print specific lines
sed -n '5,10p' file.txt                     # Print lines 5-10 only
sed -n '/ERROR/p' app.log                   # Print lines containing ERROR

# Insert / append
sed '5i\inserted line' file.txt             # Insert before line 5
sed '5a\appended line' file.txt             # Append after line 5

# Real DevOps: Replace placeholders in config templates
sed -i "s/{{APP_VERSION}}/${VERSION}/g" deployment.yaml
sed -i "s/{{NAMESPACE}}/${NAMESPACE}/g" deployment.yaml
sed -i "s/{{REPLICAS}}/${REPLICAS}/g" deployment.yaml

# Update a config value
sed -i 's/^worker_processes.*/worker_processes auto;/' /etc/nginx/nginx.conf
sed -i 's/^#.*AllowUsers.*/AllowUsers deploy ansible/' /etc/ssh/sshd_config
```

**DevOps scenario:** CI/CD deployment script. Before applying Kubernetes manifest: `sed -i "s/image: myapp:latest/image: myapp:${GIT_SHA}/" deployment.yaml` — injects the exact commit SHA as the image tag.

---

### `sort` — Sort Lines

**What it does:** Sorts lines of text alphabetically, numerically, or by specific fields.

**Why DevOps engineers need it:** Sorting log output, finding top N results, deduplication pipeline.

**Syntax:**
```bash
sort file.txt                       # Alphabetical sort
sort -r file.txt                    # Reverse order
sort -n numbers.txt                 # Numeric sort (not lexicographic)
sort -rn numbers.txt                # Numeric reverse sort (largest first)
sort -k2 file.txt                   # Sort by 2nd field
sort -k2 -n file.txt                # Sort by 2nd field numerically
sort -t',' -k3 -n data.csv         # CSV, sort by 3rd column numerically
sort -u file.txt                    # Sort + deduplicate (unique)

# Pipeline patterns
cat /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
# → Top 10 IP addresses by request count (the canonical log analysis pipeline)
```

**DevOps scenario:** After deployment, `grep "response_time" app.log | awk '{print $NF}' | sort -n | tail -10` finds the 10 slowest response times to identify performance regressions.

---

### `uniq` — Report or Filter Repeated Lines

**What it does:** Filters out or reports duplicate adjacent lines. Almost always used with `sort`.

**Why DevOps engineers need it:** Counting occurrences (frequency analysis), deduplication, finding unique values in logs.

**Syntax:**
```bash
sort file.txt | uniq               # Remove duplicates
sort file.txt | uniq -c            # Count occurrences
sort file.txt | uniq -d            # Show ONLY duplicates
sort file.txt | uniq -u            # Show ONLY unique (non-repeated) lines

# The canonical frequency analysis pattern:
# command | sort | uniq -c | sort -rn | head -N
cat access.log | awk '{print $9}' | sort | uniq -c | sort -rn
# HTTP status code frequency: 200=1043, 404=87, 500=12
```

---

### `wc` — Word Count

**What it does:** Counts lines, words, and characters in files or stdin.

**Why DevOps engineers need it:** Count log entries, check file sizes, verify record counts.

**Syntax:**
```bash
wc -l file.txt                  # Count lines (most used)
wc -w file.txt                  # Count words
wc -c file.txt                  # Count bytes
wc -m file.txt                  # Count characters

# Pipeline use
grep "ERROR" app.log | wc -l    # Count error lines
kubectl get pods -n production | grep Running | wc -l   # Count running pods
ls /etc/nginx/conf.d/ | wc -l  # How many nginx config files?
```

**DevOps scenario:** Alert system: `ERROR_COUNT=$(grep -c "ERROR" app.log); if [ $ERROR_COUNT -gt 100 ]; then alert "Too many errors: $ERROR_COUNT"; fi`

---

### `cut` — Extract Fields from Lines

**What it does:** Extracts specific columns or character ranges from each line.

**Why DevOps engineers need it:** Quick field extraction from delimited files (CSV, TSV, colon-separated). Faster than `awk` for simple extractions.

**Syntax:**
```bash
cut -d':' -f1 /etc/passwd           # Get usernames (field 1, colon delimiter)
cut -d',' -f2,4 data.csv            # Fields 2 and 4 from CSV
cut -c1-10 file.txt                 # First 10 characters of each line
cut -d' ' -f1 access.log            # First field (IP) from access log

# Practical
kubectl get pods | cut -d' ' -f1   # Just pod names
df -h | cut -d' ' -f1              # Just filesystem names
```

---

### `tr` — Translate or Delete Characters

**What it does:** Translates, squeezes, or deletes characters from stdin.

**Why DevOps engineers need it:** Case conversion, character replacement, removing unwanted characters from pipeline output.

**Syntax:**
```bash
echo "hello" | tr 'a-z' 'A-Z'      # Lowercase to uppercase: HELLO
echo "hello" | tr 'A-Z' 'a-z'      # Uppercase to lowercase
echo "a:b:c" | tr ':' ','           # Replace : with ,
echo "  hello   " | tr -s ' '       # Squeeze multiple spaces to one
echo "hello123" | tr -d '0-9'       # Delete all digits
cat /proc/1/environ | tr '\0' '\n'  # Null-separated to newline-separated
```

**DevOps scenario:** `kubectl get pods -o json | python3 -c "import sys,json; [print(p['metadata']['name']) for p in json.load(sys.stdin)['items']]"` — or more simply with `tr` to clean up output in pipelines.

---

### `xargs` — Build and Execute Commands from stdin

**What it does:** Reads items from stdin and passes them as arguments to a command.

**Why DevOps engineers need it:** Run a command on each item in a list. The glue between `find`/`grep` output and commands that act on files. Critical for batch operations.

**Syntax:**
```bash
find /tmp -name "*.tmp" | xargs rm           # Delete all .tmp files
cat pod_names.txt | xargs kubectl delete pod # Delete listed pods
echo "host1 host2 host3" | xargs -n1 ping -c1  # Ping each host once
find . -name "*.py" | xargs grep "import requests"  # Find Python files using requests
ls *.log | xargs -I{} cp {} /backup/{}       # Copy each log to backup

# Parallel execution
cat hosts.txt | xargs -P4 -I{} ssh {} uptime  # Run uptime on 4 hosts simultaneously

# With delimiter
echo "file1:file2:file3" | xargs -d':' ls -l
```

**DevOps scenario:** Bulk Kubernetes operation — `kubectl get pods -n production --no-headers | grep CrashLoopBackOff | awk '{print $1}' | xargs kubectl delete pod -n production` — delete all crash-looping pods in one command.

---

---

## Category 5 — File Permissions and Ownership

---

### `chmod` — Change File Mode (Permissions)

**What it does:** Changes file permissions — who can read, write, or execute a file.

**Why DevOps engineers need it:** Security. SSH keys must be `600` or SSH refuses to use them. Scripts must be `755` to be executable. Config files with passwords should be `640`. Wrong permissions = security vulnerabilities or broken functionality.

**Understanding permissions:**
```
Permission string: -rwxr-xr--
                   │├──┤├──┤├──┤
                   │user│grp│other
                   type

r = read    = 4
w = write   = 2
x = execute = 1

7 = rwx (4+2+1)
6 = rw- (4+2)
5 = r-x (4+1)
4 = r-- (4)
0 = --- (0)
```

**Syntax:**
```bash
chmod 755 script.sh          # rwxr-xr-x: owner full, group+other read+execute
chmod 644 config.conf        # rw-r--r--: owner read+write, others read only
chmod 600 ~/.ssh/id_rsa      # rw-------: owner only (SSH key requirement)
chmod 700 ~/.ssh/            # rwx------: owner only (SSH directory requirement)
chmod 640 /etc/app/secret.conf  # rw-r-----: owner+group read, no other access
chmod +x deploy.sh           # Add execute for everyone
chmod -R 755 /var/www/html/  # Recursive: set permissions on directory tree
chmod u+x,g-w,o-r file.txt  # Symbolic: add execute for user, remove write for group, remove read for other
```

**DevOps scenario:** `find /etc/app -type f -name "*.conf" -exec chmod 640 {} \;` — fix permissions on all config files. `find /opt/scripts -type f -name "*.sh" -exec chmod 755 {} \;` — make all shell scripts executable.

---

### `chown` — Change Owner and Group

**What it does:** Changes the user and/or group ownership of a file or directory.

**Why DevOps engineers need it:** Applications run as specific users (nginx, postgres, www-data). Files must be owned by the right user or the application can't read/write them.

**Syntax:**
```bash
chown alice file.txt                   # Change owner to alice
chown alice:developers file.txt        # Change owner and group
chown :developers file.txt             # Change only group
chown -R www-data:www-data /var/www/  # Recursive: change owner of entire tree
chown --reference=ref.txt target.txt  # Set same ownership as ref.txt
```

**DevOps scenario:** After deploying application files: `chown -R appuser:appgroup /opt/myapp/` ensures the application process (running as appuser) can read its own files.

---

### `umask` — Default Permission Mask

**What it does:** Sets the default permissions for newly created files and directories.

**Why DevOps engineers need it:** Ensures files created by automation scripts have appropriate permissions from the start, without requiring explicit chmod afterwards.

**Syntax:**
```bash
umask                   # Show current umask
umask 022               # Standard: files=644, dirs=755
umask 027               # Stricter: files=640, dirs=750 (group can read, others cannot)
umask 077               # Most restrictive: files=600, dirs=700 (owner only)
```

---

---

## Category 6 — Process Management

---

### `ps` — Process Status

**What it does:** Shows currently running processes.

**Why DevOps engineers need it:** Finding what processes are running, their PIDs, resource usage, and owner. Essential for debugging (is the app even running?), killing hung processes, and security (unexpected processes running?).

**Syntax:**
```bash
ps aux                     # All processes, all users, with details (most used)
ps aux | grep nginx        # Find nginx processes
ps aux | grep -v grep | grep nginx  # Cleaner: exclude the grep itself
ps -ef                     # Full format listing
ps -u www-data             # Processes owned by www-data
ps aux --sort=-%cpu | head -10  # Top 10 by CPU usage
ps aux --sort=-%mem | head -10  # Top 10 by memory usage

# Get PID of a process
pgrep nginx                # Get PID(s) of nginx
pidof nginx                # Same
ps -C nginx -o pid,comm    # Formatted output
```

**DevOps scenario:** Deploy health check — `if ! ps aux | grep -q "[n]ginx"; then echo "ALERT: nginx is not running"; fi` — the `[n]` trick prevents matching the grep process itself.

---

### `kill` and `pkill` — Send Signals to Processes

**What it does:** Sends signals to processes. SIGTERM (15) asks gracefully, SIGKILL (9) forces immediate termination.

**Why DevOps engineers need it:** Restart processes, force-kill hung processes, send reload signals to nginx/services without downtime.

**Syntax:**
```bash
kill PID                   # Send SIGTERM (graceful shutdown)
kill -9 PID                # Send SIGKILL (force kill — last resort)
kill -HUP PID              # Send SIGHUP (reload config — nginx, sshd)
kill -15 PID               # Explicit SIGTERM

pkill nginx                # Kill all nginx processes by name
pkill -9 -u alice          # Force kill all processes by user alice
pkill -HUP nginx           # Reload nginx without downtime

# Graceful first, then force
kill -15 $PID
sleep 5
kill -9 $PID 2>/dev/null || true   # Force if still alive

# Get PID and kill in one line
kill $(pgrep nginx)
```

**DevOps scenario:** Nginx config reload without downtime: `kill -HUP $(pgrep -f "nginx: master")` sends SIGHUP to the nginx master process, which reloads config and gracefully drains existing connections.

---

### `top` and `htop` — Real-Time Process Monitor

**What it does:** Shows real-time system resource usage — CPU, memory, load average, and per-process stats.

**Why DevOps engineers need it:** First response to a performance alert. CPU spike? Memory leak? `top` shows it instantly. `htop` is the same but coloured and more interactive.

**Syntax:**
```bash
top                        # Interactive real-time monitor

# top keyboard shortcuts:
# P  — sort by CPU (default)
# M  — sort by memory
# k  — kill a process (type PID)
# q  — quit
# 1  — show individual CPU cores
# u username — filter by user

htop                       # Coloured, mouse-enabled version (may need install)

# Non-interactive snapshots
top -bn1 | head -20        # Single snapshot, first 20 lines
top -bn1 | grep "Cpu"      # Just CPU stats
```

**DevOps scenario:** CPU alert fires. SSH to server. `top` sorted by CPU (press P) immediately shows which process is consuming CPU. Sort by memory (press M) to check memory pressure.

---

### `nohup` and `&` — Run Processes in Background

**What it does:** `&` runs a process in the background. `nohup` makes it survive after you log out.

**Why DevOps engineers need it:** Running long operations (database migrations, deployments) that must continue even if your SSH connection drops.

**Syntax:**
```bash
command &                          # Run in background, dies when terminal closes
nohup command &                    # Run in background, survives logout
nohup ./deploy.sh > deploy.log 2>&1 &   # Background with logging

# Check background jobs
jobs                               # List background jobs in current shell
fg %1                              # Bring job 1 to foreground
bg %1                              # Send job 1 to background

# More reliable: use screen or tmux
screen -S deployment               # Create named screen session
# Run your command
# Ctrl+A, D to detach
screen -r deployment               # Reattach later
```

---

---

## Category 7 — System Monitoring and Performance

---

### `top` / `vmstat` / `iostat` — Performance Monitoring Suite

**`vmstat` — Virtual Memory Statistics:**
```bash
vmstat 1 5              # Update every 1 second, 5 times
vmstat -s               # Summary statistics
# Key columns: r=run queue, b=blocked, swpd=swap, free, si/so=swap in/out
# us=user CPU%, sy=system CPU%, id=idle%, wa=I/O wait%
# High wa = disk I/O bottleneck
# High si/so = memory pressure causing swap
```

**`iostat` — I/O Statistics:**
```bash
iostat -x 1             # Extended I/O stats, every second
iostat -x 1 5           # 5 intervals
# Key columns: %util (disk busy %), await (avg wait ms), tps (transactions/sec)
# %util > 80% = disk is the bottleneck
```

**`free` — Memory Usage:**
```bash
free -h                 # Human-readable memory stats
free -h -s 2            # Update every 2 seconds

# Output:
#         total   used    free    shared  buff/cache  available
# Mem:    15Gi    8.2Gi   1.1Gi   512Mi   5.7Gi       6.8Gi
# Swap:   2.0Gi   0B      2.0Gi

# Key: "available" is what's actually usable (free + cache that can be released)
# Don't panic at low "free" — Linux uses RAM for cache aggressively
```

**`uptime` — System Load:**
```bash
uptime
# Output: 14:30:01 up 45 days, 3:21, 2 users, load average: 0.15, 0.18, 0.12
#                                                              1min  5min  15min
# Load average = average number of processes waiting for CPU
# Load > number of CPU cores = overloaded
# Check CPU count: nproc or grep -c processor /proc/cpuinfo
```

---

### `df` — Disk Free Space

**What it does:** Shows disk space usage for filesystems.

**Why DevOps engineers need it:** Disk full = application crash, database corruption, CI failure. This is one of the most important health metrics. Checked in every on-call scenario.

**Syntax:**
```bash
df -h                   # Human-readable (all filesystems)
df -h /                 # Just root filesystem
df -h /var/log          # Where /var/log is mounted
df -i                   # Inode usage (disk can be full on inodes even with free space!)
df -hT                  # Include filesystem type

# Alert script
USAGE=$(df -h / | awk 'NR==2 {print $5}' | tr -d '%')
if [ $USAGE -gt 85 ]; then
    echo "ALERT: Root disk at ${USAGE}%"
fi
```

**DevOps scenario:** `df -h` shows 95% usage. `du -sh /var/log/* | sort -rh | head -10` (see below) finds which directories are largest.

---

### `du` — Disk Usage

**What it does:** Shows disk space used by files and directories.

**Why DevOps engineers need it:** Finding what's eating your disk when `df` shows high usage. `df` tells you the disk is full. `du` tells you what filled it.

**Syntax:**
```bash
du -sh /var/log              # Total size of /var/log
du -sh /var/log/*            # Size of each item in /var/log
du -sh /* 2>/dev/null        # Size of each top-level directory
du -sh * | sort -rh          # Current directory, sorted by size (largest first)
du -h --max-depth=2 /var     # Two levels deep under /var
du -ah /var/log | sort -rh | head -20  # Top 20 largest files/dirs

# Find top space consumers
du -sh /var/log/* 2>/dev/null | sort -rh | head -10
```

**DevOps scenario:** Disk at 90%. `du -sh /* 2>/dev/null | sort -rh | head -5` shows which top-level directories are largest. Then drill down: `du -sh /var/* 2>/dev/null | sort -rh | head -5`.

---

### `lsof` — List Open Files

**What it does:** Lists all open files and the processes that opened them. In Linux, everything is a file — sockets, pipes, devices, regular files.

**Why DevOps engineers need it:** Find what process has a file locked. Find what ports a process is listening on. Troubleshoot "file is in use" errors. Check network connections of a specific process.

**Syntax:**
```bash
lsof                           # All open files (huge output)
lsof -p PID                    # Files opened by specific process
lsof -u www-data               # Files opened by user www-data
lsof /var/log/app.log          # What process has this file open?
lsof -i                        # All network connections
lsof -i :80                    # What's listening on port 80?
lsof -i :8080 -i :443          # Multiple ports
lsof -i TCP:80 -sTCP:LISTEN    # Only listening sockets on port 80
lsof -c nginx                  # Files opened by processes named nginx

# Find deleted files still holding disk space
lsof | grep deleted            # Large deleted files still held open by processes
# Common scenario: a log file was rm'd but process still writing = disk not freed
```

**DevOps scenario:** `df -h` shows disk at 98% but `du -sh /*` only accounts for 40%. `lsof | grep "(deleted)"` — a process is writing to a deleted log file. The solution: restart the process to release the file handle.

---

---

## Category 8 — Disk and Storage

---

### `mount` / `umount` — Mount and Unmount Filesystems

**What it does:** Attaches (mounts) storage devices and network filesystems to directory paths.

**Why DevOps engineers need it:** Attaching EBS volumes, NFS shares, and examining disk contents. Cloud VMs often have additional data volumes to mount.

**Syntax:**
```bash
mount                          # Show all currently mounted filesystems
mount | grep /dev/sd           # Show physical disk mounts
mount /dev/sdb1 /mnt/data      # Mount a disk partition
mount -t nfs server:/share /mnt/nfs  # Mount NFS share
umount /mnt/data               # Unmount

# AWS: Mount an EBS volume
lsblk                          # List block devices
mkfs.ext4 /dev/xvdf            # Format (only if new disk!)
mount /dev/xvdf /data          # Mount it
# Add to /etc/fstab for persistence
```

---

### `lsblk` — List Block Devices

**What it does:** Shows all block devices (disks, partitions, LVM volumes) in a tree format.

**Why DevOps engineers need it:** Quick overview of disks and their mount points. Essential when working with cloud VM storage.

**Syntax:**
```bash
lsblk                          # Tree view of all block devices
lsblk -f                       # Include filesystem type and UUID
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT  # Custom columns
```

---

---

## Category 9 — Networking

---

### `curl` — Transfer Data from URLs

**What it does:** Makes HTTP, HTTPS, FTP, and other protocol requests. The Swiss Army knife of network testing.

**Why DevOps engineers need it:** Test API endpoints, health checks, download files, test SSL certificates, debug headers. Used in every deployment script for health verification.

**Syntax:**
```bash
curl https://api.example.com/health         # Simple GET request
curl -v https://api.example.com             # Verbose (shows headers)
curl -I https://api.example.com             # Headers only (HEAD request)
curl -L https://example.com                 # Follow redirects
curl -o output.json https://api.example.com # Save response to file
curl -s https://api.example.com             # Silent (no progress bar)
curl -f https://api.example.com             # Fail with non-200 status (for scripts!)
curl -w "%{http_code}" -o /dev/null https://api.example.com   # Just status code
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' https://api.example.com/endpoint     # POST JSON
curl -H "Authorization: Bearer ${TOKEN}" https://api.example.com  # With auth header
curl --max-time 10 https://api.example.com  # Timeout after 10 seconds
curl -k https://self-signed.example.com     # Ignore SSL errors (testing only!)
curl --cert client.pem --key client.key https://mtls.example.com  # Client cert auth

# DevOps health check pattern
HTTP_CODE=$(curl -s -o /dev/null -w "%{http_code}" https://api.example.com/health)
if [ "$HTTP_CODE" != "200" ]; then
    echo "Health check failed: HTTP $HTTP_CODE"
    exit 1
fi
```

**DevOps scenario:** Post-deployment verification: `curl -sf https://api.production.com/health | jq .status` — if this doesn't return `"healthy"`, trigger a rollback.

---

### `wget` — Download Files

**What it does:** Downloads files from web URLs. Better than curl for downloading large files or recursive website downloads.

**Why DevOps engineers need it:** Downloading binaries, packages, and artifacts in scripts where curl is not available or wget's file-saving behavior is more convenient.

**Syntax:**
```bash
wget https://example.com/file.tar.gz              # Download file
wget -O custom_name.tar.gz https://example.com/file.tar.gz  # Custom filename
wget -q https://example.com/file.tar.gz           # Quiet mode
wget -P /tmp/ https://example.com/file.tar.gz    # Save to specific directory
wget --progress=bar https://example.com/large.iso # Show progress bar

# Continue interrupted download
wget -c https://example.com/large.tar.gz
```

---

### `netstat` / `ss` — Network Statistics

**What it does:** Shows network connections, listening ports, routing tables. `ss` is the modern replacement for `netstat`.

**Why DevOps engineers need it:** Check what ports are listening, verify connections, troubleshoot "port already in use" errors, security audits.

**Syntax:**
```bash
# ss (modern, faster)
ss -tlnp                    # TCP, Listening, Numeric, Process (most used)
ss -tlnp | grep :80         # Who's listening on port 80?
ss -an                      # All connections numeric
ss -tp                      # All TCP with process names
ss -s                       # Summary statistics

# netstat (legacy, may not be installed)
netstat -tlnp               # TCP listening with process
netstat -an | grep ESTABLISHED | wc -l  # Count established connections
netstat -tulnp              # TCP + UDP listening

# Check specific port
ss -tlnp | grep ':3306'     # Is MySQL listening?
ss -tlnp | grep ':6379'     # Is Redis listening?
```

**DevOps scenario:** `ss -tlnp` on a new server shows which ports are open. Any unexpected ports open could indicate a security issue or misconfiguration.

---

### `ping` — Test Network Connectivity

**What it does:** Sends ICMP echo requests to test basic network reachability and round-trip time.

**Why DevOps engineers need it:** First-step network troubleshooting. Is the host reachable? Is there packet loss?

**Syntax:**
```bash
ping google.com             # Continuous ping (Ctrl+C to stop)
ping -c 4 google.com        # Send exactly 4 packets
ping -c 4 -i 0.5 google.com # Send 4 packets 0.5 seconds apart
ping -W 3 slow-host.com     # Timeout after 3 seconds per packet

# Quick connectivity test in scripts
if ping -c 1 -W 2 database.internal &>/dev/null; then
    echo "Database host is reachable"
else
    echo "Cannot reach database host!"
fi
```

---

### `nslookup` / `dig` — DNS Lookup

**What it does:** Queries DNS to resolve hostnames to IP addresses and inspect DNS records.

**Why DevOps engineers need it:** Debug DNS issues in deployments. Verify new DNS records have propagated. Check which IP a hostname resolves to (critical during blue-green and canary releases).

**Syntax:**
```bash
nslookup api.mycompany.com              # Simple DNS lookup
dig api.mycompany.com                   # Detailed DNS lookup
dig api.mycompany.com A                 # A record (IPv4)
dig api.mycompany.com CNAME             # CNAME record
dig api.mycompany.com MX                # Mail records
dig @8.8.8.8 api.mycompany.com         # Query Google's DNS (check propagation)
dig +short api.mycompany.com            # Just the IP address
dig +trace api.mycompany.com            # Full DNS resolution path

# Check if DNS has propagated (compare different DNS servers)
dig @8.8.8.8 +short api.mycompany.com
dig @1.1.1.1 +short api.mycompany.com
dig @your-dns-server +short api.mycompany.com
```

**DevOps scenario:** After DNS change, verify propagation: `for dns in 8.8.8.8 1.1.1.1 9.9.9.9; do echo -n "$dns: "; dig @$dns +short api.mycompany.com; done`

---

### `traceroute` / `tracepath` — Trace Network Path

**What it does:** Shows the route packets take through the network to reach a destination, with latency at each hop.

**Why DevOps engineers need it:** Diagnose where network latency comes from. Identify if routing is going through unexpected paths.

**Syntax:**
```bash
traceroute google.com        # Trace path to google
tracepath google.com         # Alternative (no root required)
traceroute -n google.com    # Numeric (skip DNS lookups, faster)
```

---

### `ip` — IP Address and Routing

**What it does:** Shows and manages network interfaces, IP addresses, and routing tables. Modern replacement for `ifconfig`.

**Why DevOps engineers need it:** Check server IP configuration, add/remove IP addresses, check routes.

**Syntax:**
```bash
ip addr                     # Show all network interfaces and IPs
ip addr show eth0           # Show specific interface
ip route                    # Show routing table
ip route show default       # Show default gateway
ip link                     # Show link-layer info

# Legacy (still works on older systems)
ifconfig -a                 # All interfaces
ifconfig eth0               # Specific interface
```

---

---

## Category 10 — Archive and Compression

---

### `tar` — Tape Archive (Create and Extract Archives)

**What it does:** Creates and extracts archive files (.tar, .tar.gz, .tar.bz2, .tar.xz).

**Why DevOps engineers need it:** Packaging deployments, creating backups, transferring directory trees. The primary archive tool in Linux.

**Syntax:**
```bash
# CREATE archives
tar -czf archive.tar.gz directory/           # Create gzip-compressed archive
tar -czf archive.tar.gz file1 file2 dir/    # Multiple files/dirs
tar -cjf archive.tar.bz2 directory/         # bzip2 compression (smaller, slower)
tar -cf archive.tar directory/              # No compression

# EXTRACT archives
tar -xzf archive.tar.gz                     # Extract gzip archive here
tar -xzf archive.tar.gz -C /opt/myapp/     # Extract to specific directory
tar -xf archive.tar.bz2                     # Extract bzip2 (auto-detects)
tar -xf archive.tar.xz                      # Extract xz

# LIST contents without extracting
tar -tzf archive.tar.gz                     # List files in gzip archive
tar -tf archive.tar                         # List files in uncompressed archive

# Flags mnemonic: c=create, x=extract, t=list, z=gzip, j=bzip2, f=file, v=verbose

# Create deployment package
tar -czf myapp-v1.2.3.tar.gz \
    --exclude='*.pyc' \
    --exclude='__pycache__' \
    --exclude='.git' \
    myapp/
```

**DevOps scenario:** Build artifact packaging in CI: `tar -czf build-${GIT_SHA}.tar.gz dist/ --exclude='*.map'` — package build artifacts for deployment without source maps.

---

### `gzip` / `gunzip` — Compress Individual Files

**What it does:** Compresses files using gzip algorithm. Replaces the original file with a compressed `.gz` version.

**Syntax:**
```bash
gzip large_log.log          # Compress → large_log.log.gz (original deleted)
gzip -k large_log.log       # Keep original file
gzip -d file.gz             # Decompress (same as gunzip)
gunzip file.gz              # Decompress
zcat file.gz                # Read compressed file without decompressing
zgrep "ERROR" file.gz       # Grep compressed file without decompressing
```

**DevOps scenario:** `zgrep "ERROR" /var/log/app.log.*.gz | wc -l` — count errors across all archived (compressed) log files without decompressing them.

---

### `zip` / `unzip`

**What it does:** Creates and extracts ZIP archives (Windows-compatible format).

**Syntax:**
```bash
zip archive.zip file1 file2         # Create zip
zip -r archive.zip directory/       # Recursive (include directory)
unzip archive.zip                   # Extract
unzip -d /dest/ archive.zip        # Extract to specific directory
unzip -l archive.zip               # List contents
```

---

---

## Category 11 — User and Permission Management

---

### `whoami` / `id` / `who`

**What it does:** Shows current user identity and group memberships.

**Why DevOps engineers need it:** Verify you're running as the right user. Check if a user has the required group memberships. Essential before running privileged commands.

**Syntax:**
```bash
whoami                      # Current username
id                          # uid, gid, and all groups
id alice                    # Groups for a specific user
who                         # Who is logged in
w                           # Who is logged in and what they're doing
last                        # Login history
last | head -20             # Recent login history
```

---

### `sudo` — Execute as Another User (Usually Root)

**What it does:** Runs a command with elevated privileges (usually as root) based on sudoers configuration.

**Why DevOps engineers need it:** Most administrative tasks require root. `sudo` provides controlled elevated access without sharing the root password.

**Syntax:**
```bash
sudo command                # Run as root
sudo -u postgres psql       # Run as postgres user
sudo -l                     # List what sudo commands you're allowed to run
sudo su -                   # Switch to root shell (not recommended)
sudo -i                     # Root login shell
sudo !!                     # Re-run last command with sudo

# Check sudoers configuration
sudo cat /etc/sudoers
sudo visudo                 # Safely edit sudoers
```

---

### `useradd` / `usermod` / `userdel`

**What it does:** Manage user accounts.

**Why DevOps engineers need it:** Creating service accounts for applications, adding deployment users, managing access.

**Syntax:**
```bash
# Create user
useradd -m -s /bin/bash deploy          # Create with home dir and bash shell
useradd -r -s /sbin/nologin appuser     # System user (no shell, no login)

# Modify user
usermod -aG docker alice                # Add alice to docker group
usermod -aG sudo alice                  # Give alice sudo access
usermod -L alice                        # Lock account (disable login)
usermod -U alice                        # Unlock account

# Delete user
userdel alice                           # Delete user (keep home dir)
userdel -r alice                        # Delete user AND home directory

# Change password
passwd alice                            # Set password for alice
passwd -l alice                         # Lock password (disable password login)
```

---

---

## Category 12 — Package Management

---

### `apt` / `apt-get` — Debian/Ubuntu Package Manager

**What it does:** Installs, removes, and manages software packages on Debian-based systems (Ubuntu, Debian, Mint).

**Why DevOps engineers need it:** Installing tools on servers and in Docker images. Ubuntu is the dominant Linux in cloud environments.

**Syntax:**
```bash
apt update                              # Update package index (always before install)
apt install -y nginx                    # Install nginx (-y for non-interactive)
apt install -y nginx postgresql redis  # Install multiple packages
apt remove nginx                        # Remove (keep config)
apt purge nginx                         # Remove + delete config
apt autoremove                          # Remove unused dependencies
apt upgrade                             # Upgrade all installed packages
apt-cache search nginx                  # Search for package
apt-cache show nginx                    # Show package info
dpkg -l | grep nginx                    # Check if nginx is installed
dpkg -l                                 # List all installed packages
```

---

### `yum` / `dnf` — Red Hat/CentOS Package Manager

**What it does:** Same as apt but for Red Hat-based systems (CentOS, RHEL, Fedora, Amazon Linux).

**Syntax:**
```bash
yum install -y nginx                    # Install (CentOS 7)
dnf install -y nginx                    # Install (CentOS 8+, Fedora)
yum update                              # Update all packages
yum remove nginx                        # Remove
yum list installed | grep nginx         # Check installation
yum info nginx                          # Package info
yum search nginx                        # Search
rpm -qa | grep nginx                    # List installed rpm packages matching nginx
```

---

---

## Category 13 — Services and Systemd

---

### `systemctl` — Control systemd Services

**What it does:** Manages systemd services — start, stop, restart, enable on boot, view status and logs.

**Why DevOps engineers need it:** Almost all services on modern Linux (nginx, postgres, docker, ssh) are managed by systemd. `systemctl` is how you control them. Essential for every production server.

**Syntax:**
```bash
# Service control
systemctl start nginx               # Start service
systemctl stop nginx                # Stop service
systemctl restart nginx             # Stop + start
systemctl reload nginx              # Reload config without restart (graceful)
systemctl status nginx              # Service status + recent logs
systemctl enable nginx              # Auto-start on boot
systemctl disable nginx             # Don't auto-start on boot
systemctl is-active nginx           # Is it running? (ok/inactive)
systemctl is-enabled nginx          # Will it start on boot?
systemctl is-failed nginx           # Did it fail?

# View all services
systemctl list-units --type=service         # All services
systemctl list-units --type=service --state=failed  # Failed services

# Reload systemd after editing service files
systemctl daemon-reload

# Full status with logs
systemctl status nginx -l --no-pager    # Full output, no truncation
```

**DevOps scenario:** After deployment, `systemctl status myapp` instantly shows if the service started, its PID, active time, and the last few log lines — first check when a deployment seems to fail.

---

### `journalctl` — View systemd Logs

**What it does:** Queries and displays logs from the systemd journal.

**Why DevOps engineers need it:** Centralised logs for all systemd services. More powerful than reading individual log files — supports time filtering, service filtering, and live following.

**Syntax:**
```bash
journalctl                              # All logs (huge)
journalctl -u nginx                     # Logs for nginx service
journalctl -u nginx -f                  # Follow nginx logs live
journalctl -u nginx -n 100             # Last 100 lines of nginx logs
journalctl -u nginx --since "1 hour ago"  # Last hour of nginx logs
journalctl -u nginx --since "2024-01-15 14:00" --until "2024-01-15 15:00"
journalctl -p err                       # Only error priority and above
journalctl -p err -u nginx              # Nginx errors only
journalctl -b                           # Logs since last boot
journalctl -b -1                        # Logs from previous boot
journalctl --disk-usage                 # Journal size on disk
```

**DevOps scenario:** Service fails after deploy. `journalctl -u myapp -n 50 --no-pager` shows the last 50 log lines including the startup sequence and any errors.


---

---

## Category 14 — SSH and Remote Operations

---

### `ssh` — Secure Shell

**What it does:** Opens an encrypted terminal session to a remote machine. The primary tool for accessing servers.

**Why DevOps engineers need it:** Servers don't have monitors. SSH is how you access them. Every server management task, every incident response, every deployment verification on a remote machine uses SSH.

**Syntax:**
```bash
ssh user@hostname                           # Connect
ssh user@192.168.1.10                      # Connect by IP
ssh -i ~/.ssh/mykey.pem user@host          # Use specific private key
ssh -p 2222 user@host                      # Non-standard port
ssh -v user@host                           # Verbose (debug connection issues)
ssh -L 8080:localhost:80 user@host         # Local port forward (tunnel)
ssh -R 8080:localhost:80 user@host         # Remote port forward
ssh -J jump@jumphost user@target           # Jump host (bastion)
ssh -o StrictHostKeyChecking=no user@host  # Skip host key check (automation)
ssh -N -L 5432:db-internal:5432 user@bastion  # Tunnel to internal database

# Run remote command without interactive session
ssh user@host "df -h"
ssh user@host "systemctl status nginx"
ssh user@host "tail -100 /var/log/app.log"
```

**~/.ssh/config — Essential time-saver:**
```
Host production
    HostName 10.0.1.50
    User ubuntu
    IdentityFile ~/.ssh/prod_key.pem
    Port 22

Host bastion
    HostName bastion.mycompany.com
    User ec2-user
    IdentityFile ~/.ssh/aws_key.pem

Host internal-db
    HostName 10.0.2.100
    User postgres
    ProxyJump bastion
    LocalForward 5432 localhost:5432
```

```bash
# After config, just:
ssh production
ssh internal-db
```

**DevOps scenario:** Accessing an RDS database that has no public access: `ssh -N -L 5432:mydb.xxx.rds.amazonaws.com:5432 ec2-user@bastion-host` — creates a tunnel so your local machine can connect to `localhost:5432` which forwards to the internal RDS endpoint.

---

### `scp` — Secure Copy

**What it does:** Copies files between machines over SSH.

**Why DevOps engineers need it:** Transferring configs, logs, and deployment artifacts to/from servers.

**Syntax:**
```bash
scp file.txt user@host:/tmp/                    # Copy TO remote
scp user@host:/var/log/app.log ./              # Copy FROM remote
scp -r directory/ user@host:/opt/myapp/        # Copy directory recursively
scp -i ~/.ssh/key.pem file.txt user@host:/tmp/ # With specific key
scp -P 2222 file.txt user@host:/tmp/           # Non-standard port (note uppercase -P)
```

---

### `rsync` — Fast, Incremental File Transfer

**What it does:** Synchronises files between locations efficiently — only transfers the differences. Works locally or over SSH.

**Why DevOps engineers need it:** Deploying applications, syncing S3-like structures, creating efficient backups. Much faster than `scp` for repeated transfers because it only copies what changed.

**Syntax:**
```bash
rsync -av source/ dest/                          # Sync local dirs (verbose)
rsync -avz source/ user@host:/dest/             # Sync to remote over SSH
rsync -avz --delete source/ dest/               # Mirror (delete files not in source)
rsync -avz --progress source/ user@host:/dest/  # Show progress
rsync -avz --exclude='*.log' src/ dest/         # Exclude log files
rsync -avz --exclude-from='.rsyncignore' src/ dest/  # Use exclude file
rsync -n source/ dest/                           # Dry run (preview changes)
rsync -avz -e "ssh -i ~/.ssh/key.pem -p 2222" src/ user@host:/dest/  # Custom SSH options
```

**DevOps scenario:** Deploying a web application: `rsync -avz --delete --exclude='.git' --exclude='node_modules' dist/ deploy@webserver:/var/www/html/` — fast incremental deployment, only transferring changed files.

---

### `ssh-keygen` — Generate SSH Keys

**What it does:** Generates cryptographic key pairs for SSH authentication.

**Why DevOps engineers need it:** Setting up passwordless SSH (required for automation), creating keys for CI/CD pipelines, rotating compromised keys.

**Syntax:**
```bash
ssh-keygen -t ed25519 -C "deploy@mycompany.com"    # Generate Ed25519 key (recommended)
ssh-keygen -t rsa -b 4096 -C "deploy@company.com"  # RSA 4096-bit (for older systems)
ssh-keygen -t ed25519 -f ~/.ssh/deploy_key          # Specify filename
ssh-keygen -y -f ~/.ssh/key.pem                     # Extract public key from private key

# Copy public key to remote server
ssh-copy-id user@hostname                            # Copy default public key
ssh-copy-id -i ~/.ssh/deploy_key.pub user@hostname  # Copy specific key
```

---

---

## Category 15 — Shell and Scripting Essentials

---

### `echo` and `printf` — Print Text

**What it does:** Outputs text to stdout.

**Why DevOps engineers need it:** Debugging scripts, printing status messages, writing to files.

**Syntax:**
```bash
echo "Hello World"
echo -n "No newline"           # No trailing newline
echo -e "Tab:\t newline:\n"   # Interpret escape sequences
echo $HOME                     # Print variable value
echo "Value: ${VAR:-default}" # With default value

printf "%-20s %5d\n" "CPU:" 85  # Formatted output (like C printf)
printf "Host: %s Port: %d\n" "$HOST" "$PORT"
```

---

### `export` — Set Environment Variables

**What it does:** Sets an environment variable that child processes inherit.

**Why DevOps engineers need it:** Passing configuration to processes, setting up environment for scripts, temporary credential injection.

**Syntax:**
```bash
export VAR=value                    # Set and export
export PATH="$PATH:/opt/myapp/bin" # Add to PATH
export -p                           # List all exported variables
unset VAR                           # Remove variable

# Common DevOps exports
export KUBECONFIG=~/.kube/config
export AWS_PROFILE=production
export TF_VAR_environment=production
```

---

### `source` / `.` — Execute Script in Current Shell

**What it does:** Runs a script in the current shell (not a subshell), so variable assignments and environment changes persist.

**Why DevOps engineers need it:** Activating virtual environments, loading environment files, applying shell configuration changes without opening a new terminal.

**Syntax:**
```bash
source ~/.bashrc                    # Reload bash configuration
source venv/bin/activate            # Activate Python virtual environment
source .env                         # Load environment variables from .env file
. ~/.bashrc                         # Same as source (POSIX shorthand)

# Load .env file in script
source .env  # exports all KEY=VALUE pairs
# Or more safely:
set -a; source .env; set +a         # Export everything in .env automatically
```

---

### `env` — Run Command with Modified Environment

**What it does:** Prints or modifies the environment for a command.

**Why DevOps engineers need it:** Injecting environment variables for a single command, running processes in a clean environment, debugging environment issues.

**Syntax:**
```bash
env                                         # Print all environment variables
env | grep AWS                              # Find AWS-related variables
env VAR=value command                       # Run command with extra variable
env -i PATH=$PATH command                   # Run in nearly empty environment

# Debug: what does a process see?
cat /proc/PID/environ | tr '\0' '\n'       # All env vars of running process
```

---

### `tee` — Read from stdin and Write to File AND stdout

**What it does:** Splits output — writes to a file AND continues passing it through to stdout.

**Why DevOps engineers need it:** Logging script output while still seeing it on screen. Saving command output while piping it to another command.

**Syntax:**
```bash
command | tee output.log                    # Show + save to file
command | tee output.log | grep "ERROR"    # Show filtered + save all
command | tee -a output.log                # Append to file
./deploy.sh 2>&1 | tee deploy.log          # Log + show stdout+stderr
```

**DevOps scenario:** `terraform apply 2>&1 | tee terraform-apply-$(date +%Y%m%d_%H%M%S).log` — see the apply output live AND save a timestamped log for audit purposes.

---

### `watch` — Repeat a Command Periodically

**What it does:** Runs a command every N seconds and shows updated output.

**Why DevOps engineers need it:** Monitoring deployments, watching resource usage, observing DNS propagation — without running the same command manually every few seconds.

**Syntax:**
```bash
watch kubectl get pods -n production        # Update every 2 seconds (default)
watch -n 5 kubectl get pods                 # Update every 5 seconds
watch -d kubectl get pods                   # Highlight differences between updates
watch -n 1 "df -h / | tail -1"            # Watch disk usage
watch -n 1 "ps aux --sort=-%cpu | head -5" # Watch top CPU processes
```

**DevOps scenario:** During a rolling deployment: `watch -n 2 "kubectl get pods -n production | grep api"` — watch pods cycle from Running to Terminating to Running with the new version.

---

### Redirection Operators — `>`, `>>`, `<`, `2>`, `&>`

**What it does:** Controls where command input and output goes.

**Why DevOps engineers need it:** Capturing output to files, suppressing errors, separating stdout from stderr, piping data between commands.

**Syntax:**
```bash
command > output.txt           # Redirect stdout to file (overwrite)
command >> output.txt          # Redirect stdout to file (append)
command < input.txt            # Read stdin from file
command 2> errors.txt          # Redirect stderr to file
command 2>&1                   # Redirect stderr to stdout (merge)
command > output.txt 2>&1      # Redirect both stdout+stderr to file
command &> output.txt          # Same (bash shorthand)
command > /dev/null            # Discard stdout
command 2>/dev/null            # Discard stderr (suppress error messages)
command > /dev/null 2>&1       # Discard all output

# Practical examples
./install.sh > install.log 2>&1           # Save all output to log
./deploy.sh 2>/dev/null                    # Run silently (suppress errors)
curl -sf https://api.com/health &>/dev/null && echo "UP" || echo "DOWN"
```

---

### `|` — The Pipe Operator

**What it does:** Connects the stdout of one command to the stdin of the next. The foundation of Unix philosophy: small tools doing one thing, combined via pipes.

**Why DevOps engineers need it:** Every complex log analysis, every monitoring command, every data transformation uses pipes. Pipes are what make the command line infinitely powerful.

```bash
# The anatomy of a pipe:
command1 | command2 | command3 | command4

# Each command receives the previous command's output as input

# Real examples:
ps aux | grep nginx | grep -v grep              # Find nginx processes
cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
kubectl get pods -n production | grep Running | wc -l
journalctl -u nginx | grep -i error | tail -20
```

---

### `&&`, `||`, and `;` — Command Chaining

**What it does:** Controls whether subsequent commands run based on the exit code of the previous one.

**Why DevOps engineers need it:** Conditional execution is essential for safe automation — only run the next step if the previous one succeeded.

**Syntax:**
```bash
cmd1 && cmd2        # Run cmd2 ONLY if cmd1 succeeds (exit 0)
cmd1 || cmd2        # Run cmd2 ONLY if cmd1 FAILS (exit non-0)
cmd1 ; cmd2         # Always run cmd2, regardless of cmd1 result

# DevOps patterns
terraform init && terraform plan && terraform apply   # Each step only if previous succeeded
./test.sh && ./deploy.sh || ./rollback.sh            # Test → deploy, or rollback
mkdir -p /opt/myapp && cd /opt/myapp && git clone ... # Create, navigate, clone
```

---

---

## Category 16 — Git for DevOps

---

### `git` — Version Control

**Why DevOps engineers need it:** All Infrastructure as Code (Terraform, Helm, Kubernetes manifests), all deployment scripts, all pipeline configs live in Git. DevOps without Git is not DevOps.

**Core commands every DevOps engineer must know:**
```bash
# Setup
git config --global user.name "Alice Smith"
git config --global user.email "alice@company.com"

# Daily operations
git clone https://github.com/org/repo.git     # Clone repository
git status                                     # What files changed?
git diff                                       # What exactly changed?
git add .                                      # Stage all changes
git add -p                                     # Interactively stage hunks
git commit -m "fix: nginx config for prod"    # Commit with message
git push origin main                           # Push to remote
git pull                                       # Pull latest changes

# Branching
git branch                                     # List branches
git checkout -b feature/new-config             # Create + switch to branch
git checkout main                              # Switch branch
git merge feature/new-config                  # Merge branch
git rebase main                               # Rebase (linear history)

# History
git log --oneline -10                          # Last 10 commits
git log --oneline --graph --all               # Commit graph
git show abc123                               # Show specific commit

# Undoing
git restore file.txt                          # Discard working dir changes
git reset HEAD~1                              # Undo last commit (keep changes)
git reset --hard HEAD~1                       # Undo last commit (DISCARD changes)
git revert abc123                             # Create new commit undoing abc123

# Tags (for releases)
git tag -a v1.2.3 -m "Release v1.2.3"
git push origin v1.2.3
git describe --tags --abbrev=0                # Get latest tag

# Useful for CI/CD
git rev-parse --short HEAD                    # Short commit SHA
git branch --show-current                     # Current branch name
git log --oneline HEAD..origin/main           # Commits not yet pulled
```

---

---

## Category 17 — Docker CLI

---

### `docker` — Container Management

**Why DevOps engineers need it:** Containers are the deployment unit in modern infrastructure. `docker` is how you build, run, inspect, and debug containers.

```bash
# Images
docker images                              # List local images
docker pull nginx:alpine                   # Pull image from registry
docker build -t myapp:v1.0 .             # Build from Dockerfile
docker push myregistry/myapp:v1.0        # Push to registry
docker rmi myapp:v1.0                    # Remove image
docker image prune -a                    # Remove all unused images

# Containers
docker run -d --name myapp -p 8080:80 nginx:alpine   # Run detached
docker run -it ubuntu:22.04 bash                      # Interactive shell
docker run --rm nginx:alpine nginx -t                 # Test nginx config, delete container
docker ps                              # Running containers
docker ps -a                           # All containers (including stopped)
docker stop myapp                      # Graceful stop
docker start myapp                     # Start stopped container
docker restart myapp                   # Restart
docker rm myapp                        # Delete container
docker rm -f myapp                     # Force delete running container

# Debugging
docker logs myapp                      # Container logs
docker logs myapp -f --tail=50        # Follow last 50 lines
docker exec -it myapp bash             # Shell into running container
docker exec -it myapp sh               # Shell (Alpine/minimal images)
docker inspect myapp                   # Full container details (JSON)
docker stats                           # Real-time resource usage
docker top myapp                       # Processes in container

# Docker Compose
docker compose up -d                   # Start all services
docker compose down                    # Stop and remove containers
docker compose logs -f api             # Follow service logs
docker compose exec api bash           # Shell into service
docker compose ps                      # Service status

# Networking
docker network ls                      # List networks
docker network inspect bridge         # Inspect network

# Volumes
docker volume ls                       # List volumes
docker volume inspect myvolume         # Volume details

# System
docker system df                       # Disk usage
docker system prune                    # Clean up stopped containers, unused images
docker system prune -a                 # Clean everything unused
```

**DevOps scenario:** Container is misbehaving in production. `docker exec -it myapp sh` drops you inside the container. `ps aux` shows what's running. `env` shows environment variables. `cat /etc/nginx/nginx.conf` shows the config. You diagnose without restarting.

---

---

## Category 18 — Kubernetes — kubectl

---

### `kubectl` — Kubernetes Command-Line Tool

**Why DevOps engineers need it:** Kubernetes is the dominant container orchestration platform. `kubectl` is how you deploy, manage, debug, and operate Kubernetes workloads. Non-negotiable for any DevOps engineer working with containers.

```bash
# Cluster context
kubectl config get-contexts                          # List available clusters
kubectl config use-context production-cluster        # Switch cluster
kubectl config current-context                       # Which cluster?

# Pods
kubectl get pods -n production                       # List pods
kubectl get pods -n production -o wide               # With node and IP info
kubectl get pods -A                                  # All namespaces
kubectl describe pod myapp-abc123 -n production      # Full pod details
kubectl logs myapp-abc123 -n production              # Pod logs
kubectl logs myapp-abc123 -n production -f           # Follow logs
kubectl logs myapp-abc123 -n production --previous   # Previous container logs
kubectl exec -it myapp-abc123 -n production -- bash  # Shell into pod
kubectl delete pod myapp-abc123 -n production        # Delete pod (will restart)

# Deployments
kubectl get deployments -n production
kubectl describe deployment myapp -n production
kubectl set image deployment/myapp myapp=myapp:v2.0 -n production   # Update image
kubectl rollout status deployment/myapp -n production                # Watch rollout
kubectl rollout history deployment/myapp -n production               # History
kubectl rollout undo deployment/myapp -n production                  # Rollback
kubectl rollout restart deployment/myapp -n production               # Rolling restart
kubectl scale deployment myapp --replicas=5 -n production            # Scale

# Services and Networking
kubectl get services -n production
kubectl get endpoints -n production
kubectl port-forward service/myapp 8080:80 -n production  # Local port forward

# Config and Secrets
kubectl get configmaps -n production
kubectl describe configmap app-config -n production
kubectl get secrets -n production
kubectl get secret db-secret -o jsonpath='{.data.password}' | base64 -d  # Decode secret

# Apply and Delete
kubectl apply -f deployment.yaml                      # Apply manifest
kubectl apply -f ./k8s/                              # Apply all manifests in directory
kubectl delete -f deployment.yaml                    # Delete resources from manifest

# Resource Usage
kubectl top nodes                                     # Node resource usage
kubectl top pods -n production                        # Pod resource usage

# Events (crucial for debugging)
kubectl get events -n production --sort-by='.lastTimestamp'    # Recent events
kubectl get events -n production --field-selector type=Warning  # Warnings only

# Namespaces
kubectl get namespaces
kubectl create namespace staging

# Nodes
kubectl get nodes
kubectl describe node worker-node-1
kubectl drain node-1 --ignore-daemonsets             # Prepare for maintenance
kubectl uncordon node-1                               # Return to service

# Output formats
kubectl get pods -o yaml                              # YAML format
kubectl get pods -o json                              # JSON format
kubectl get pods -o jsonpath='{.items[*].metadata.name}'  # Extract field
kubectl get pods --no-headers                         # No column headers

# Labels and selectors
kubectl get pods -l app=myapp                         # Filter by label
kubectl label pod mypod env=production               # Add label
```

**DevOps scenario — complete debugging flow:**
```bash
# 1. Pod in CrashLoopBackOff
kubectl get pods -n production | grep CrashLoop

# 2. See why it's crashing
kubectl logs myapp-abc123 -n production --previous

# 3. Full details
kubectl describe pod myapp-abc123 -n production | tail -30

# 4. Check events for scheduling issues
kubectl get events -n production --sort-by='.lastTimestamp' | tail -20

# 5. Check resource limits
kubectl describe pod myapp-abc123 | grep -A 5 Limits

# 6. Shell in (if it's running long enough)
kubectl exec -it myapp-abc123 -n production -- sh
```

---

---

## Category 19 — Security and Audit

---

### `openssl` — Cryptographic Tool

**What it does:** A comprehensive cryptography toolkit — inspect certificates, generate keys, test SSL connections.

**Why DevOps engineers need it:** SSL certificate issues cause outages. `openssl` lets you inspect, test, and diagnose certificates before they expire or cause failures.

**Syntax:**
```bash
# Check certificate expiry (most common use)
echo | openssl s_client -servername mysite.com -connect mysite.com:443 2>/dev/null \
    | openssl x509 -noout -dates

# Check certificate expiry quickly
openssl s_client -connect mysite.com:443 2>/dev/null \
    | openssl x509 -noout -enddate

# Inspect a certificate file
openssl x509 -in cert.pem -text -noout
openssl x509 -in cert.pem -noout -subject -dates

# Test HTTPS connection
openssl s_client -connect mysite.com:443

# Generate a private key
openssl genrsa -out key.pem 4096

# Check if private key matches certificate
openssl x509 -noout -modulus -in cert.pem | md5sum
openssl rsa -noout -modulus -in key.pem | md5sum
# Both must produce the same hash

# Alert: days until certificate expires
EXPIRY=$(echo | openssl s_client -servername mysite.com -connect mysite.com:443 2>/dev/null \
    | openssl x509 -noout -enddate | cut -d= -f2)
DAYS=$(( ( $(date -d "$EXPIRY" +%s) - $(date +%s) ) / 86400 ))
echo "Certificate expires in $DAYS days"
```

---

### `iptables` — Firewall Rules

**What it does:** Configures Linux kernel's built-in firewall (netfilter).

**Why DevOps engineers need it:** Understanding server firewall rules, troubleshooting blocked connections, temporary firewall rules.

**Syntax:**
```bash
iptables -L -n -v                    # List all rules (numeric, verbose)
iptables -L INPUT -n -v              # Just INPUT chain
iptables -A INPUT -p tcp --dport 80 -j ACCEPT   # Allow HTTP
iptables -A INPUT -s 10.0.0.0/8 -j ACCEPT       # Allow from subnet
iptables -D INPUT 3                  # Delete rule 3 from INPUT

# Modern alternative: ufw (Ubuntu)
ufw status
ufw allow 80/tcp
ufw allow from 10.0.0.0/8 to any port 5432

# Modern alternative: firewalld (CentOS/RHEL)
firewall-cmd --list-all
firewall-cmd --add-service=http
```

---

### `fail2ban` and `last` — Security Audit

```bash
# Audit failed logins
last                                    # Recent logins
lastb                                   # Failed login attempts
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn
# Top IPs with failed login attempts

# Check for suspicious processes
ps aux | awk '{print $11}' | sort -u   # Unique process names
lsof -i -n | grep LISTEN              # Listening network ports

# Check scheduled tasks (cron)
crontab -l                              # Current user's crontab
cat /etc/crontab                        # System crontab
ls /etc/cron.d/                        # System cron jobs

# Find recently modified files (possible intrusion)
find / -mtime -1 -type f -newer /var/log/auth.log 2>/dev/null | grep -v proc | head -20

# Check sudo history
grep sudo /var/log/auth.log | tail -20
```

---

---

## Category 20 — DevOps One-Liners and Power Combos

> These are the combinations that separate experienced engineers from beginners. Each combines multiple tools to solve a real problem.

---

### Log Analysis One-Liners

```bash
# Count HTTP status codes from nginx access log
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -rn
# 1043 200
#   87 404
#   12 500
#    3 502

# Top 10 IPs by request count
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head -10

# Top 10 slowest endpoints (nginx with $request_time in log)
awk '{print $NF, $7}' /var/log/nginx/access.log | sort -rn | head -10

# Count errors per hour
grep "ERROR" /var/log/app.log | awk -F: '{print $1":"$2":00"}' | sort | uniq -c
# Group errors by hour to see if there's a spike pattern

# Find the request that generated most errors
grep " 500 " /var/log/nginx/access.log | awk '{print $7}' | sort | uniq -c | sort -rn | head -10

# Monitor error rate in real time
watch -n 5 "tail -1000 /var/log/app.log | grep -c ERROR"
```

---

### System Health One-Liners

```bash
# Everything at once: CPU, memory, disk, load
echo "=== System Health ===" && \
echo "Load: $(uptime | awk -F'load average:' '{print $2}')" && \
echo "CPU: $(top -bn1 | grep "Cpu(s)" | awk '{print $2}')%" && \
echo "Memory: $(free -h | awk '/^Mem/ {print $3"/"$2}')" && \
echo "Disk: $(df -h / | awk 'NR==2 {print $5" used"}')"

# Find and kill a process by name
kill $(pgrep -f "hung_process_name")

# Check all services and their status
systemctl list-units --type=service --state=running --no-pager | grep -v systemd

# Find what's eating disk space
find / -xdev -type f -size +100M -exec ls -lh {} \; 2>/dev/null | sort -k5 -rh

# Check open file descriptor count per process
lsof 2>/dev/null | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
```

---

### Kubernetes Power Commands

```bash
# All pods not in Running state
kubectl get pods -A | grep -v "Running\|Completed\|NAME"

# Pods with restart count > 0
kubectl get pods -A | awk '$5 > 0 {print}' | column -t

# Quick shell into first pod matching a label
kubectl exec -it $(kubectl get pod -l app=myapi -o jsonpath='{.items[0].metadata.name}') -- bash

# Delete all evicted pods
kubectl get pods -A | grep Evicted | awk '{print "kubectl delete pod " $2 " -n " $1}' | bash

# Get all Docker images running in cluster
kubectl get pods -A -o jsonpath='{range .items[*]}{.spec.containers[*].image}{"\n"}{end}' | sort -u

# Check resource requests vs limits for all pods
kubectl get pods -A -o custom-columns='NAMESPACE:.metadata.namespace,NAME:.metadata.name,CPU_REQ:.spec.containers[*].resources.requests.cpu,MEM_REQ:.spec.containers[*].resources.requests.memory'

# Watch deployment rollout progress
kubectl rollout status deployment/myapp -n production -w

# Get logs from all pods matching a label
kubectl logs -l app=myapi -n production --prefix=true --tail=50
```

---

### Network Debugging One-Liners

```bash
# Check if port is open from current machine
nc -zv hostname 3306 && echo "Port open" || echo "Port closed"
timeout 3 bash -c "</dev/tcp/hostname/3306" && echo "Port open" || echo "Port closed"

# Watch network connections in real time
watch -n 1 "ss -s"

# Most active network connections
ss -tnp | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -rn | head -10

# Check DNS resolution time
time dig google.com &>/dev/null

# Test HTTP response time
time curl -sf https://api.mycompany.com/health -o /dev/null

# Monitor bandwidth usage
iftop          # Real-time bandwidth (may need install)
nethogs        # Bandwidth per process (may need install)
```

---

### Deployment and Automation One-Liners

```bash
# Wait for a service to be ready (with timeout)
timeout 300 bash -c 'while ! curl -sf http://myservice/health; do sleep 5; done' && echo "Ready" || echo "Timed out"

# Deploy and immediately tail logs
kubectl set image deployment/myapi myapi=myapp:v2.0 -n production && \
kubectl rollout status deployment/myapi -n production && \
kubectl logs -f -l app=myapi -n production

# Check if process is running, start if not
pgrep nginx || systemctl start nginx

# Atomic config replace with backup
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.$(date +%Y%m%d_%H%M%S).bak && \
cp nginx.conf.new /etc/nginx/nginx.conf && \
nginx -t && \
systemctl reload nginx || \
(cp /etc/nginx/nginx.conf.$(date +%Y%m%d)*.bak /etc/nginx/nginx.conf && echo "Rollback!")

# SSH to all hosts in a list and run a command
while IFS= read -r host; do
    echo "=== $host ===" && ssh -o ConnectTimeout=5 "$host" "uptime" 2>/dev/null || echo "UNREACHABLE"
done < hosts.txt
```

---

### Text Processing Power Combos

```bash
# Extract value from JSON without jq
grep -o '"version": "[^"]*"' package.json | awk -F'"' '{print $4}'

# Count unique values in a field
awk -F: '{print $3}' /etc/passwd | sort -n | uniq | wc -l   # Unique UIDs

# Find duplicate lines
sort file.txt | uniq -d

# Compare two files, show only differences
diff <(sort file1.txt) <(sort file2.txt)

# Find and replace across multiple files (in-place)
find . -name "*.yml" -exec sed -i 's/image: myapp:latest/image: myapp:v1.2.3/g' {} \;

# Extract specific lines from large file efficiently
sed -n '10000,10100p' huge_file.txt  # Lines 10000-10100 (faster than head+tail)

# Count words/lines/chars
wc -l file.txt; wc -w file.txt; wc -c file.txt

# Merge two sorted files removing duplicates
sort -m -u file1.txt file2.txt
```

---

---

## Quick Reference Card

### The 20 Most Reached-For Commands in Production

| Command | When You Reach For It |
|---|---|
| `tail -f /var/log/app.log` | Watching logs in real time |
| `grep -r "pattern" /var/log/` | Finding error in logs |
| `ps aux \| grep process` | Is the process running? |
| `df -h` | Is the disk full? |
| `du -sh * \| sort -rh` | What's eating my disk? |
| `netstat -tlnp / ss -tlnp` | What ports are listening? |
| `top / htop` | What's using CPU/memory? |
| `systemctl status service` | Why did the service fail? |
| `journalctl -u service -n 100` | Recent service logs |
| `kubectl get pods -A` | Are all pods running? |
| `kubectl logs pod -f` | What is this pod doing? |
| `kubectl exec -it pod -- bash` | Debug inside container |
| `docker logs container -f` | Docker container logs |
| `curl -sf http://service/health` | Is the service responding? |
| `ssh -i key.pem user@host` | Access a remote server |
| `find / -name "filename"` | Where is this file? |
| `lsof -i :8080` | What's on this port? |
| `chmod 755 script.sh` | Make script executable |
| `kill -HUP $(pgrep nginx)` | Reload nginx config |
| `history \| grep terraform` | What did I run earlier? |

---

## Official Documentation Reference

| Resource | URL |
|---|---|
| Linux man pages online | https://man7.org/linux/man-pages/ |
| GNU Bash Manual | https://www.gnu.org/software/bash/manual/ |
| Linux Command Reference | https://linuxcommand.org/ |
| kubectl Reference | https://kubernetes.io/docs/reference/kubectl/ |
| Docker CLI Reference | https://docs.docker.com/engine/reference/commandline/ |
| SS (socket statistics) | https://man7.org/linux/man-pages/man8/ss.8.html |
| systemd/systemctl | https://systemd.io/ |
| OpenSSL docs | https://www.openssl.org/docs/ |

---

## How to Get Better at Linux Commands

```
1. Use the man page before Google:  man grep, man awk, man find
2. Use --help for quick reference:  grep --help | head -30
3. Build the muscle memory:         Use the command line for EVERYTHING
4. Keep a notes file:               Save useful one-liners you discover
5. Understand what you type:        Never blindly copy commands from the internet
6. Learn the pipe pattern:          output | filter | transform | format
7. Practice in a VM first:          Test destructive commands (rm, chmod -R) safely
8. Read other people's scripts:     Open source Bash scripts are a goldmine
9. Use shellcheck:                  shellcheck myscript.sh catches common bugs
10. The man page has examples:      Always read to the bottom
```

---

*Built on: GNU/Linux documentation · man pages · kubectl reference · Docker CLI reference · Production DevOps patterns*
