# 🐧 Linux Administration: Complete Learning Guide
### From Zero to Linux Administrator

---

> **How to use this guide:** Work through each module in order. Don't skip ahead — each module builds on the last. Practice every command in a real terminal. Understanding comes from doing, not just reading.

---

## 📚 TABLE OF CONTENTS

1. [Module 1 — What is Linux?](#module-1)
2. [Module 2 — Getting Started: Installation & Interface](#module-2)
3. [Module 3 — The Linux Filesystem](#module-3)
4. [Module 4 — Essential Commands (Navigation & Files)](#module-4)
5. [Module 5 — Users, Groups & Permissions](#module-5)
6. [Module 6 — Text Editing & File Manipulation](#module-6)
7. [Module 7 — Processes & System Monitoring](#module-7)
8. [Module 8 — Package Management](#module-8)
9. [Module 9 — Networking Fundamentals](#module-9)
10. [Module 10 — Shell Scripting](#module-10)
11. [Module 11 — Storage, Disks & Filesystems](#module-11)
12. [Module 12 — Services & Systemd](#module-12)
13. [Module 13 — Logs & Troubleshooting](#module-13)
14. [Module 14 — Security & Firewall](#module-14)
15. [Module 15 — SSH & Remote Administration](#module-15)
16. [Module 16 — Cron Jobs & Automation](#module-16)
17. [Module 17 — Advanced Administration Topics](#module-17)
18. [Practice Labs & Projects](#practice-labs)
19. [Quick Reference Cheat Sheet](#cheat-sheet)

---

<a name="module-1"></a>
## MODULE 1 — What is Linux?

### What You'll Learn
- What Linux is and why it matters
- The history of Linux
- Linux distributions (distros)
- Where Linux is used

---

### 1.1 What is Linux?

Linux is an **open-source operating system kernel** created by **Linus Torvalds** in 1991. An operating system manages hardware (CPU, memory, storage) and allows software to run on it.

Think of it like this:
- **Hardware** = the physical computer
- **Kernel** = the translator between hardware and software
- **Linux** = the kernel that ties everything together

Linux is:
- **Free** — no license cost
- **Open Source** — anyone can read, modify, and share the code
- **Stable** — servers run for years without rebooting
- **Secure** — fine-grained control over who can do what
- **Everywhere** — Android, web servers, supercomputers, routers

---

### 1.2 A Brief History

| Year | Event |
|------|-------|
| 1969 | Unix created at Bell Labs |
| 1983 | GNU Project started by Richard Stallman |
| 1991 | Linus Torvalds releases Linux kernel 0.01 |
| 1994 | Linux 1.0 released |
| 2003 | Red Hat Enterprise Linux (RHEL) becomes dominant in enterprise |
| 2004 | Ubuntu launched, making Linux accessible for beginners |
| Today | Linux runs 96%+ of the world's top websites |

---

### 1.3 Linux Distributions

A **distribution (distro)** = Linux kernel + software packages + package manager + desktop environment.

| Distribution | Best For | Package Manager |
|-------------|----------|-----------------|
| **Ubuntu** | Beginners, general use | apt |
| **Debian** | Stability, servers | apt |
| **CentOS / AlmaLinux** | Enterprise servers | dnf / yum |
| **Red Hat Enterprise (RHEL)** | Corporate enterprise | dnf |
| **Fedora** | Cutting-edge features | dnf |
| **Arch Linux** | Advanced users, custom | pacman |
| **Kali Linux** | Security / penetration testing | apt |

> **Recommendation for beginners:** Start with **Ubuntu 22.04 LTS** or **AlmaLinux 9** (good for admin learning).

---

### 1.4 Where Linux is Used

- **Web Servers** — Apache, Nginx run on Linux
- **Cloud Computing** — AWS, Google Cloud, Azure all run on Linux
- **Databases** — MySQL, PostgreSQL
- **Android** — Linux kernel at the core
- **Supercomputers** — 100% of Top500 run Linux
- **Networking** — Routers, switches, firewalls
- **IoT Devices** — Raspberry Pi, smart devices

---

### ✅ Module 1 Checkpoint
Before moving on, you should be able to answer:
1. What is the difference between the kernel and the operating system?
2. Name 3 popular Linux distributions and their use cases.
3. Why is Linux dominant in servers and the cloud?

---

<a name="module-2"></a>
## MODULE 2 — Getting Started: Installation & Interface

### What You'll Learn
- How to install Linux (or use a VM)
- Understanding the terminal
- Basic terminal navigation

---

### 2.1 Setting Up Your Linux Environment

You have several options:

**Option A: Virtual Machine (Recommended for beginners)**
1. Download [VirtualBox](https://www.virtualbox.org/) (free)
2. Download Ubuntu 22.04 LTS ISO from ubuntu.com
3. Create a new VM → allocate 2GB RAM, 20GB disk
4. Boot from ISO → follow installer

**Option B: WSL2 on Windows 10/11**
```powershell
# Run in PowerShell as Administrator
wsl --install
```
Then install Ubuntu from Microsoft Store.

**Option C: Cloud (Free Tier)**
- AWS EC2 Free Tier
- Google Cloud Free Tier
- Oracle Cloud (always free tier — generous)

---

### 2.2 The Terminal (Your Most Important Tool)

The terminal (also called: console, shell, command line) is where you will spend most of your time as a Linux admin. Don't fear it — learn to love it.

**Opening the terminal:**
- Ubuntu GUI: Press `Ctrl + Alt + T`
- SSH: `ssh user@server_ip`

**Understanding the prompt:**
```
username@hostname:~$
│        │       │ └── $ = regular user, # = root (superuser)
│        │       └──── ~ = current directory (home)
│        └──────────── hostname = computer name
└───────────────────── username = who you are
```

---

### 2.3 The Shell

The shell is the interpreter that reads your commands. The most common shell is **Bash** (Bourne Again Shell).

```bash
# Check which shell you're using
echo $SHELL

# Check the shell version
bash --version
```

Other shells you may encounter:
- **sh** — original Bourne shell
- **zsh** — popular with power users
- **fish** — user-friendly, modern

---

### 2.4 Getting Help

```bash
# Manual pages — the most important help system
man ls           # Read the manual for 'ls' command
man -k keyword   # Search for commands by keyword

# Quick help
ls --help        # Short help for most commands
whatis ls        # One-line description
info ls          # Alternative to man (more detail sometimes)

# Navigate man pages
# Arrow keys to scroll
# 'q' to quit
# '/' to search, 'n' for next match
```

> **Pro Tip:** `man man` reads the manual for the manual itself.

---

### ✅ Module 2 Checkpoint
1. Install Linux in a VM or WSL2
2. Open a terminal and run `echo "Hello, Linux!"`
3. Read `man ls` and find what the `-l` flag does

---

<a name="module-3"></a>
## MODULE 3 — The Linux Filesystem

### What You'll Learn
- The Linux directory tree
- What each directory is used for
- Absolute vs relative paths

---

### 3.1 Everything is a File

In Linux, **everything is treated as a file**:
- Regular files (text, binaries, images)
- Directories (folders)
- Devices (hard drives, USB) — stored in `/dev`
- Network sockets, pipes — also files

---

### 3.2 The Filesystem Hierarchy

Linux uses a single directory tree starting at `/` (root).

```
/                   ← Root of the entire filesystem
├── bin/            ← Essential user binaries (ls, cp, mv)
├── sbin/           ← System binaries (for admin tasks: fdisk, iptables)
├── boot/           ← Boot files (kernel, GRUB bootloader)
├── dev/            ← Device files (sda, tty, null, random)
├── etc/            ← System configuration files
├── home/           ← User home directories
│   ├── alice/
│   └── bob/
├── lib/            ← Shared libraries
├── media/          ← Removable media (USB drives, CDs)
├── mnt/            ← Temporary mount points
├── opt/            ← Optional/third-party software
├── proc/           ← Virtual filesystem (running processes info)
├── root/           ← Home directory for root user
├── run/            ← Runtime data (PIDs, sockets)
├── srv/            ← Data for services (web server files)
├── sys/            ← Virtual filesystem (kernel/hardware info)
├── tmp/            ← Temporary files (cleared on reboot)
├── usr/            ← Secondary hierarchy (most user programs)
│   ├── bin/        ← User commands (python, git, vim)
│   ├── lib/        ← Libraries for /usr/bin
│   └── local/      ← Locally installed software
└── var/            ← Variable data (logs, mail, databases)
    ├── log/        ← System logs
    └── www/        ← Web server root
```

---

### 3.3 Key Directories to Know

| Directory | Purpose | Examples |
|-----------|---------|---------|
| `/etc` | Configuration files | `/etc/passwd`, `/etc/nginx/nginx.conf` |
| `/var/log` | Log files | `/var/log/syslog`, `/var/log/auth.log` |
| `/home` | User files | `/home/john/Documents` |
| `/tmp` | Temporary files | Cleared on reboot |
| `/proc` | Kernel/process info | `/proc/cpuinfo`, `/proc/meminfo` |
| `/dev` | Device files | `/dev/sda` (disk), `/dev/null` |

---

### 3.4 Paths: Absolute vs Relative

**Absolute path** — starts from root `/`, always works regardless of where you are:
```bash
/home/alice/documents/report.txt
```

**Relative path** — relative to your current location:
```bash
documents/report.txt    # if you're already in /home/alice
../bob/file.txt         # go up one level, then into bob's directory
```

**Special path shortcuts:**
```bash
~       # Your home directory (/home/username)
.       # Current directory
..      # Parent directory (one level up)
-       # Previous directory (toggle back)
```

---

### ✅ Module 3 Checkpoint
```bash
# Try these commands:
ls /                    # List root directory
ls /etc | head -20      # First 20 items in /etc
cat /proc/cpuinfo       # View CPU information
cat /proc/meminfo       # View memory information
```

---

<a name="module-4"></a>
## MODULE 4 — Essential Commands (Navigation & Files)

### What You'll Learn
- Moving around the filesystem
- Creating, copying, moving, deleting files/directories
- Viewing file contents
- Searching for files

---

### 4.1 Navigation Commands

```bash
pwd                   # Print Working Directory — where am I?
ls                    # List files in current directory
ls -l                 # Long format (permissions, size, date)
ls -la                # Long format including hidden files (starting with .)
ls -lh                # Human-readable file sizes (KB, MB, GB)
ls -lt                # Sort by modification time (newest first)
ls /etc               # List a specific directory

cd /var/log           # Change Directory to /var/log
cd ~                  # Go to home directory
cd ..                 # Go up one level
cd -                  # Go to previous directory
cd                    # Go to home directory (shortcut)
```

---

### 4.2 Working with Files

```bash
# Create files
touch file.txt             # Create an empty file (or update timestamp)
touch file1.txt file2.txt  # Create multiple files

# View file contents
cat file.txt               # Print entire file
less file.txt              # Scrollable view (q to quit)
head file.txt              # First 10 lines
head -n 20 file.txt        # First 20 lines
tail file.txt              # Last 10 lines
tail -f /var/log/syslog    # Follow a file in real-time (great for logs!)

# Copy files
cp file.txt backup.txt           # Copy file
cp file.txt /tmp/                # Copy to a directory
cp -r folder/ /tmp/folder/       # Copy directory recursively
cp -p file.txt backup.txt        # Preserve permissions/timestamps

# Move / Rename files
mv file.txt newname.txt          # Rename file
mv file.txt /tmp/                # Move file to /tmp
mv folder/ /opt/newfolder/       # Move directory

# Delete files
rm file.txt                      # Delete file
rm -i file.txt                   # Delete with confirmation prompt
rm -r folder/                    # Delete directory recursively
rm -rf folder/                   # Force delete (NO confirmation) ⚠️ Be careful!

# NEVER run: rm -rf /   ← This deletes everything!
```

---

### 4.3 Working with Directories

```bash
mkdir mydir                   # Create directory
mkdir -p a/b/c                # Create nested directories (parents too)
mkdir dir1 dir2 dir3          # Create multiple directories

rmdir mydir                   # Remove EMPTY directory
rm -r mydir/                  # Remove directory and all contents
```

---

### 4.4 Wildcards (Globbing)

```bash
ls *.txt              # All .txt files
ls file?.txt          # file1.txt, file2.txt, filea.txt (one character)
ls file[123].txt      # file1.txt, file2.txt, file3.txt
ls /etc/*.conf        # All .conf files in /etc
rm temp*              # Delete everything starting with "temp"
```

---

### 4.5 Searching for Files

```bash
# find — powerful file search
find / -name "passwd"              # Search for "passwd" from root
find /home -name "*.txt"           # All .txt files in /home
find /var/log -name "*.log" -mtime -7   # Log files modified in last 7 days
find / -size +100M                 # Files larger than 100MB
find / -user alice                 # Files owned by alice
find / -perm 777                   # Files with 777 permissions
find /tmp -empty                   # Empty files/directories
find / -name "*.conf" -type f      # Only files (not directories)

# locate — faster but uses a database (update with: sudo updatedb)
locate passwd                      # Quick search
locate "*.log"                     # Search for log files

# which — where is a command?
which python3                      # /usr/bin/python3
which nginx                        # Find nginx binary location
```

---

### 4.6 Input/Output Redirection & Pipes

These are fundamental to working efficiently in Linux.

```bash
# Redirect output to a file
ls -l > files.txt              # Write output to file (overwrites)
ls -l >> files.txt             # Append output to file

# Redirect errors
ls /nonexistent 2> error.txt   # Redirect error (stderr) to file
ls /nonexistent 2>/dev/null    # Discard errors (/dev/null = trash)
ls -la > all.txt 2>&1          # Redirect both stdout and stderr

# Pipes — pass output of one command to another
ls -l | grep ".txt"            # Filter ls output for .txt files
cat /etc/passwd | wc -l        # Count lines in passwd
ps aux | grep nginx            # Find nginx process
dmesg | tail -20               # Last 20 kernel messages
history | grep ssh             # Search command history for ssh
```

---

### 4.7 Useful File Commands

```bash
wc -l file.txt         # Count lines
wc -w file.txt         # Count words
wc -c file.txt         # Count characters/bytes

sort file.txt          # Sort alphabetically
sort -n numbers.txt    # Sort numerically
sort -r file.txt       # Reverse sort
sort -u file.txt       # Sort and remove duplicates

uniq file.txt          # Remove consecutive duplicates
uniq -c file.txt       # Count occurrences

grep "pattern" file    # Search for pattern in file
grep -i "pattern" file # Case-insensitive search
grep -r "error" /var/log/  # Search recursively in directory
grep -n "pattern" file # Show line numbers
grep -v "pattern" file # Show lines NOT matching

diff file1 file2       # Show differences between files

file document.txt      # Determine file type
stat file.txt          # Detailed file info (size, permissions, timestamps)
```

---

### ✅ Module 4 Checkpoint

**Practice exercises:**
```bash
# 1. Navigate to /etc and list all .conf files
cd /etc && ls *.conf

# 2. Create this directory structure:
mkdir -p ~/practice/linux/admin

# 3. Create a file with content
echo "Hello Linux Admin" > ~/practice/test.txt
cat ~/practice/test.txt

# 4. Find all .log files modified in the last day
find /var/log -name "*.log" -mtime -1

# 5. Count how many users are in /etc/passwd
cat /etc/passwd | wc -l
```

---

<a name="module-5"></a>
## MODULE 5 — Users, Groups & Permissions

### What You'll Learn
- Linux user accounts
- Groups
- File permissions (chmod, chown, chgrp)
- The sudo system

---

### 5.1 Understanding Users

Linux is a **multi-user** operating system. Every file, process, and action belongs to a user.

**User types:**
- **Root (UID 0)** — superuser, unlimited power, use carefully
- **System users (UID 1-999)** — for services (nobody, www-data, mysql)
- **Regular users (UID 1000+)** — human accounts

```bash
# View user information
whoami                  # Who am I right now?
id                      # My UID, GID, and groups
id alice               # Info about another user
who                    # Who is logged in?
w                      # Who is logged in (detailed, with activity)
last                   # Login history
```

**Important user files:**
```bash
/etc/passwd            # User accounts (username:x:UID:GID:comment:home:shell)
/etc/shadow            # Encrypted passwords (readable only by root)
/etc/group             # Group definitions
```

```bash
# View user account details
cat /etc/passwd | grep alice
# Output: alice:x:1001:1001:Alice Smith:/home/alice:/bin/bash
# Fields:  name:pwd:uid:gid:comment:home:shell
```

---

### 5.2 Managing Users

```bash
# Create a user
sudo useradd alice                          # Create user (minimal)
sudo useradd -m -s /bin/bash alice          # With home dir and bash shell
sudo useradd -m -g developers -G sudo alice # Add to groups at creation

# Set/change password
sudo passwd alice

# Modify user
sudo usermod -s /bin/zsh alice              # Change shell
sudo usermod -aG sudo alice                 # Add to sudo group (-a = append)
sudo usermod -L alice                       # Lock account
sudo usermod -U alice                       # Unlock account
sudo usermod -e 2025-12-31 alice            # Set account expiry

# Delete user
sudo userdel alice                          # Delete user (keep home dir)
sudo userdel -r alice                       # Delete user AND home directory
```

---

### 5.3 Managing Groups

```bash
# Create group
sudo groupadd developers
sudo groupadd -g 2000 devops               # Specify GID

# Add user to group
sudo usermod -aG developers alice           # Add alice to developers
sudo gpasswd -a alice developers            # Alternative method

# Remove user from group
sudo gpasswd -d alice developers

# View groups
groups alice                               # Which groups is alice in?
cat /etc/group | grep developers           # See group members
```

---

### 5.4 File Permissions

This is **critical** knowledge. Every file has three permission sets:

```
-rwxr-xr--   1  alice   developers   4096   Jan 15 10:30   script.sh
│││││││││
│││├──┤││└── Others permissions (r--)
│││   ├──┤│└── Group permissions (r-x)
│││      ├──┤└── Owner permissions (rwx)
│││            └── File type (- = regular, d = directory, l = link)
││└── Execute
│└─── Write
└──── Read
```

**Permission values:**
| Symbol | Meaning | Numeric Value |
|--------|---------|---------------|
| r | Read | 4 |
| w | Write | 2 |
| x | Execute | 1 |
| - | No permission | 0 |

**Numeric (octal) notation:**
```
rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0+0+0 = 0

Common permission sets:
777 = rwxrwxrwx (all permissions for everyone — usually bad!)
755 = rwxr-xr-x (owner full, others read+execute — common for directories)
644 = rw-r--r-- (owner read+write, others read — common for files)
600 = rw------- (private files)
700 = rwx------ (private executables)
```

---

### 5.5 Changing Permissions

```bash
# chmod — change file mode (permissions)

# Numeric method
chmod 755 script.sh           # rwxr-xr-x
chmod 644 file.txt            # rw-r--r--
chmod 600 ~/.ssh/id_rsa       # rw------- (SSH keys MUST be this)
chmod -R 755 /var/www/html/   # Recursively set on directory

# Symbolic method (more readable)
chmod u+x script.sh           # Add execute for owner (user)
chmod g-w file.txt            # Remove write from group
chmod o-rwx secret.txt        # Remove all from others
chmod a+r file.txt            # Add read for all (a = all)
chmod u=rwx,g=rx,o=r file.txt # Set exact permissions

# chown — change ownership
sudo chown alice file.txt             # Change owner
sudo chown alice:developers file.txt  # Change owner and group
sudo chown -R www-data /var/www/      # Recursively change ownership
sudo chown :developers file.txt       # Change only group

# chgrp — change group
sudo chgrp developers file.txt
```

---

### 5.6 Special Permissions

```bash
# SUID (Set User ID) — run as file owner, not the runner
chmod u+s /usr/bin/passwd     # passwd runs as root even when run by normal user
ls -l /usr/bin/passwd         # Shows 's' in owner execute position: -rwsr-xr-x

# SGID (Set Group ID) — run as group, or new files inherit directory group
chmod g+s /shared/folder/

# Sticky bit — only owner can delete files in directory
chmod +t /tmp                 # /tmp uses sticky bit
ls -l /                       # Shows 't' at end: drwxrwxrwt

# Numeric for special permissions (prefix digit)
chmod 4755 file   # SUID + 755
chmod 2755 dir    # SGID + 755
chmod 1777 /tmp   # Sticky + 777
```

---

### 5.7 sudo — Running Commands as Root

```bash
sudo command              # Run as root
sudo -u alice command     # Run as alice
sudo -i                   # Start interactive root shell
sudo su -                 # Switch to root user

# Edit sudoers file (ALWAYS use visudo, never edit directly!)
sudo visudo

# Common sudoers entries:
alice ALL=(ALL:ALL) ALL         # alice can run any command as any user
alice ALL=(ALL) NOPASSWD: ALL   # No password required (risky!)
%developers ALL=(ALL) ALL       # Everyone in 'developers' group
alice ALL=/usr/bin/apt          # alice can only run apt as root
```

---

### ✅ Module 5 Checkpoint

```bash
# 1. Create a user 'testuser' with home directory
sudo useradd -m testuser
sudo passwd testuser

# 2. Create a group 'testers' and add testuser to it
sudo groupadd testers
sudo usermod -aG testers testuser

# 3. Create a file, check permissions, then change them
touch ~/testfile.txt
ls -l ~/testfile.txt
chmod 640 ~/testfile.txt
ls -l ~/testfile.txt

# 4. What permissions does 640 mean? Write it out:
# Owner: rw- (read + write)
# Group: r-- (read only)
# Others: --- (no access)
```

---

<a name="module-6"></a>
## MODULE 6 — Text Editing & File Manipulation

### What You'll Learn
- vim (the most important editor)
- nano (beginner-friendly editor)
- grep, sed, awk — text processing powerhouses

---

### 6.1 Vim — The Administrator's Editor

Vim is available on virtually every Linux system. As an admin, you'll SSH into servers where vim is your only option. Learn it.

**Vim has modes:**
- **Normal mode** — navigate, delete, copy (default on open)
- **Insert mode** — type text (press `i` to enter)
- **Visual mode** — select text (press `v`)
- **Command mode** — run commands (press `:`)

```bash
vim filename.txt        # Open file

# Essential vim commands (in Normal mode):
i           # Enter Insert mode (before cursor)
a           # Enter Insert mode (after cursor)
o           # New line below, enter Insert mode
Esc         # Return to Normal mode

# Navigation (Normal mode):
h j k l     # Left, Down, Up, Right (or use arrow keys)
gg          # Go to first line
G           # Go to last line
:50         # Go to line 50
w           # Jump forward one word
b           # Jump backward one word
0           # Go to start of line
$           # Go to end of line
Ctrl+f      # Page forward
Ctrl+b      # Page backward

# Editing (Normal mode):
dd          # Delete (cut) entire line
dw          # Delete word
d$          # Delete to end of line
yy          # Copy (yank) line
p           # Paste after cursor
u           # Undo
Ctrl+r      # Redo
x           # Delete character under cursor

# Search:
/pattern    # Search forward
n           # Next match
N           # Previous match
:%s/old/new/g    # Replace ALL occurrences
:%s/old/new/gc   # Replace with confirmation

# Saving / Quitting (Command mode):
:w          # Save
:q          # Quit (fails if unsaved changes)
:wq         # Save and quit
:q!         # Quit WITHOUT saving (discard changes)
:x          # Save and quit (same as :wq)

# Multiple files:
:split file2.txt    # Horizontal split
:vsplit file2.txt   # Vertical split
Ctrl+w w            # Switch between splits
```

---

### 6.2 Nano — Beginner-Friendly Editor

```bash
nano filename.txt       # Open file

# Commands (shown at bottom of screen):
Ctrl+O          # Save (WriteOut)
Ctrl+X          # Exit
Ctrl+W          # Search
Ctrl+K          # Cut line
Ctrl+U          # Paste
Ctrl+G          # Help
```

---

### 6.3 grep — Search Text

```bash
grep "error" /var/log/syslog          # Find lines containing "error"
grep -i "error" file.txt              # Case-insensitive
grep -n "error" file.txt              # Show line numbers
grep -c "error" file.txt              # Count matching lines
grep -v "error" file.txt              # Invert — lines WITHOUT "error"
grep -r "TODO" /home/alice/           # Recursive search in directory
grep -E "error|warning" file.txt      # Extended regex (multiple patterns)
grep -A 3 "error" file.txt            # 3 lines After match
grep -B 3 "error" file.txt            # 3 lines Before match
grep -w "log" file.txt                # Whole word match only
```

---

### 6.4 sed — Stream Editor

sed edits text line by line — great for automated editing.

```bash
# Substitution (replace text)
sed 's/old/new/' file.txt            # Replace first occurrence per line
sed 's/old/new/g' file.txt           # Replace ALL occurrences
sed 's/old/new/gi' file.txt          # Replace (case-insensitive)
sed -i 's/old/new/g' file.txt        # Edit file IN PLACE (modifies file!)
sed -i.bak 's/old/new/g' file.txt    # Edit in place, keep .bak backup

# Delete lines
sed '/pattern/d' file.txt            # Delete lines matching pattern
sed '5d' file.txt                    # Delete line 5
sed '5,10d' file.txt                 # Delete lines 5-10

# Print specific lines
sed -n '5p' file.txt                 # Print only line 5
sed -n '5,10p' file.txt              # Print lines 5-10

# Real-world examples:
sed -i 's/ServerName .*/ServerName production.example.com/' /etc/apache2/apache2.conf
sed -i '/^#/d' config.txt            # Remove all comment lines
```

---

### 6.5 awk — Data Processing

awk is like a mini programming language for processing text.

```bash
# Print specific fields (columns)
awk '{print $1}' file.txt            # Print column 1
awk '{print $1, $3}' file.txt        # Print columns 1 and 3
awk -F: '{print $1}' /etc/passwd     # Use ':' as delimiter, print column 1
awk -F: '{print $1, $3}' /etc/passwd # Print username and UID

# Filter with conditions
awk '$3 > 1000' /etc/passwd          # Lines where field 3 > 1000
awk '/pattern/ {print $1}' file.txt  # Print col 1 for matching lines

# Built-in variables
awk '{print NR, $0}' file.txt        # NR = line number, $0 = whole line
awk 'END {print NR}' file.txt        # Print total number of lines

# Real-world examples:
# Find users with UID > 1000 (regular users)
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd

# Sum file sizes from ls -l
ls -l | awk '{sum += $5} END {print sum}'
```

---

### ✅ Module 6 Checkpoint

```bash
# 1. Open a file in vim and practice:
vim ~/practice.txt
# Type 'i' to enter insert mode
# Type some text on multiple lines
# Press Esc, then :wq to save and quit

# 2. Use grep to find all error lines in syslog
sudo grep -i "error" /var/log/syslog | tail -20

# 3. Use sed to add a comment to a line in a file
echo "allow 192.168.1.0/24" > test.conf
sed -i 's/allow/# allow/' test.conf
cat test.conf

# 4. Use awk to print usernames from /etc/passwd
awk -F: '{print $1}' /etc/passwd
```

---

<a name="module-7"></a>
## MODULE 7 — Processes & System Monitoring

### What You'll Learn
- What processes are
- Viewing and managing processes
- Monitoring system resources (CPU, memory, disk)
- Killing processes

---

### 7.1 Understanding Processes

A **process** is a running instance of a program. Every process has:
- **PID** — Process ID (unique number)
- **PPID** — Parent Process ID
- **Owner** — which user started it
- **State** — Running, Sleeping, Zombie, Stopped

```bash
# View processes
ps                      # Your current processes
ps aux                  # ALL processes, all users, detailed
ps aux | grep nginx     # Find nginx process
ps -ef                  # Alternative format, shows parent PIDs
ps -u alice             # Processes owned by alice

# Process tree (shows parent-child relationships)
pstree
pstree -p               # With PIDs

# Get PID of a program
pidof nginx
pgrep nginx             # Find PID by name
```

---

### 7.2 Real-Time Monitoring

```bash
# top — real-time process viewer
top
# Inside top:
# k — kill a process (type PID)
# r — renice (change priority)
# q — quit
# M — sort by memory
# P — sort by CPU
# h — help

# htop — improved top (install: sudo apt install htop)
htop
# More visual, color-coded, easier to use
# F9 to kill process, F10 to quit

# Understanding top output:
# load average: 0.15, 0.20, 0.18
# Numbers = load over last 1min, 5min, 15min
# Load of 1.0 = 100% of one CPU core is busy
```

---

### 7.3 System Resource Monitoring

```bash
# Memory
free -h                 # Memory usage (human readable)
free -m                 # Memory in megabytes
cat /proc/meminfo       # Detailed memory info

# CPU
lscpu                   # CPU information
cat /proc/cpuinfo       # Detailed CPU info
mpstat                  # CPU statistics (install: sysstat)
uptime                  # System uptime and load averages

# Disk usage
df -h                   # Disk space on all filesystems
df -hT                  # Include filesystem type
du -sh /var/log         # Size of a directory
du -sh /*               # Size of all top-level directories
du -sh * | sort -h      # Sort directories by size
ncdu                    # Interactive disk usage (install separately)

# I/O
iostat                  # Disk I/O statistics
iotop                   # Top for I/O (shows which process uses disk)

# Network
netstat -tulnp          # Open ports and listening services
ss -tulnp               # Modern replacement for netstat
ip addr                 # Network interfaces and IPs
```

---

### 7.4 Managing Processes

```bash
# Kill processes
kill PID                # Send SIGTERM (graceful stop) to process
kill -9 PID             # Send SIGKILL (force kill — cannot be ignored)
kill -15 PID            # SIGTERM (same as kill PID)
killall nginx           # Kill all processes named nginx
pkill nginx             # Kill by name (pattern)

# Common signals:
# SIGTERM (15) — polite request to stop
# SIGKILL (9)  — immediate force kill
# SIGHUP (1)   — hang up, often used to reload config
# SIGSTOP (19) — pause process
# SIGCONT (18) — continue paused process

# Background/Foreground jobs
command &               # Run command in background
jobs                    # List background jobs
fg %1                   # Bring job 1 to foreground
bg %1                   # Resume job 1 in background
Ctrl+Z                  # Suspend current foreground process
Ctrl+C                  # Interrupt/kill current process

# nohup — run process that survives terminal close
nohup ./long_script.sh &
nohup ./script.sh > output.log 2>&1 &
```

---

### 7.5 Process Priority (nice)

```bash
# Priority range: -20 (highest) to 19 (lowest)
# Default = 0

nice -n 10 ./script.sh          # Start with lower priority (10)
nice -n -10 ./script.sh         # Higher priority (needs sudo)
sudo renice -n 5 -p 1234        # Change priority of running process (PID 1234)
renice -n 19 -u alice           # Lower priority of all alice's processes
```

---

### ✅ Module 7 Checkpoint

```bash
# 1. View all running processes, find your bash session
ps aux | grep bash

# 2. Check memory and disk usage
free -h
df -h

# 3. Run a background job
sleep 100 &
jobs
kill %1     # Kill job 1

# 4. Find which process is using the most CPU
ps aux --sort=-%cpu | head -5
```

---

<a name="module-8"></a>
## MODULE 8 — Package Management

### What You'll Learn
- Installing, updating, removing software
- APT (Debian/Ubuntu)
- DNF/YUM (RHEL/CentOS/AlmaLinux)

---

### 8.1 APT (Advanced Package Tool) — Debian/Ubuntu

```bash
# Update package list (do this first, always!)
sudo apt update

# Upgrade installed packages
sudo apt upgrade                    # Upgrade packages
sudo apt full-upgrade               # Upgrade + remove obsolete packages
sudo apt dist-upgrade               # Full system upgrade

# Install packages
sudo apt install nginx              # Install nginx
sudo apt install nginx curl git -y  # Install multiple, auto-confirm

# Remove packages
sudo apt remove nginx               # Remove (keep config files)
sudo apt purge nginx                # Remove + delete config files
sudo apt autoremove                 # Remove unused dependencies

# Search for packages
apt search nginx                    # Search available packages
apt show nginx                      # Detailed info about a package

# List packages
dpkg -l                             # List all installed packages
dpkg -l | grep nginx                # Check if nginx is installed
apt list --installed                # List installed packages

# Fix broken packages
sudo apt install -f                 # Fix broken dependencies
sudo dpkg --configure -a            # Reconfigure unpacked packages
```

---

### 8.2 DNF/YUM — Red Hat/CentOS/AlmaLinux

```bash
# dnf is the modern replacement for yum (use dnf)

sudo dnf update                     # Update all packages
sudo dnf install nginx              # Install nginx
sudo dnf remove nginx               # Remove nginx
sudo dnf search nginx               # Search for nginx
sudo dnf info nginx                 # Info about nginx

sudo dnf list installed             # List installed packages
sudo dnf list available             # List available packages

sudo dnf group install "Development Tools"   # Install package groups
sudo dnf autoremove                 # Remove unused dependencies

# Repositories
sudo dnf repolist                   # List enabled repositories
sudo dnf config-manager --enable epel   # Enable EPEL repo
```

---

### 8.3 Building from Source (When Packages Aren't Available)

```bash
# General steps for compiling from source:
# 1. Get the source code
wget https://example.com/program-1.0.tar.gz
tar -xzf program-1.0.tar.gz
cd program-1.0/

# 2. Configure (checks dependencies, sets install location)
./configure --prefix=/usr/local

# 3. Compile
make -j$(nproc)        # -j = parallel jobs = number of CPU cores

# 4. Install
sudo make install

# 5. Verify
which program
```

---

### ✅ Module 8 Checkpoint

```bash
# Ubuntu/Debian:
sudo apt update
sudo apt install curl htop tree
tree /etc/apt                   # See apt configuration

# Check what version was installed
apt show curl | grep Version

# Remove a package cleanly
sudo apt purge tree
sudo apt autoremove
```

---

<a name="module-9"></a>
## MODULE 9 — Networking Fundamentals

### What You'll Learn
- Network configuration
- Testing connectivity
- DNS resolution
- Ports and services

---

### 9.1 Network Interfaces

```bash
# View interfaces and IP addresses
ip addr                         # Show all interfaces and IPs
ip addr show eth0               # Show specific interface
ip link                         # Show interfaces (no IPs)

# Old command (still common):
ifconfig                        # Install: sudo apt install net-tools

# Route information
ip route                        # Routing table
ip route show                   # Same, shows default gateway

# Configure IP (temporary — lost on reboot)
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up        # Enable interface
sudo ip link set eth0 down      # Disable interface

# Permanent config: Edit /etc/netplan/*.yaml (Ubuntu)
```

---

### 9.2 Testing Connectivity

```bash
# ping — test reachability
ping google.com                 # Ping until Ctrl+C
ping -c 4 google.com            # Ping 4 times
ping -i 2 google.com            # Ping every 2 seconds

# traceroute — trace the path to a host
traceroute google.com
tracepath google.com            # Alternative (no root needed)

# DNS lookup
nslookup google.com             # DNS query
dig google.com                  # Detailed DNS query
dig google.com MX               # Get mail server records
dig @8.8.8.8 google.com         # Query specific DNS server (Google's)
host google.com                 # Simple DNS lookup

# Network connections
ss -tulnp                       # Open ports and services
netstat -tulnp                  # Same (older command)
ss -s                           # Summary statistics

# Test if a port is open
telnet 192.168.1.1 80          # Connect to port 80
nc -zv 192.168.1.1 80          # Netcat — test port
curl -I http://example.com      # HTTP request headers
```

---

### 9.3 Downloading Files

```bash
# wget — download files
wget http://example.com/file.txt
wget -O myfile.txt http://example.com/file.txt   # Save as specific name
wget -c http://example.com/large.iso              # Resume interrupted download
wget -r -np http://example.com/dir/              # Recursive download

# curl — transfer data (more flexible than wget)
curl http://example.com                           # Download to stdout
curl -o file.txt http://example.com/file.txt     # Save to file
curl -I http://example.com                        # Headers only
curl -X POST -d "data=value" http://api.example.com  # POST request
curl -u user:pass http://example.com             # Basic auth
```

---

### 9.4 /etc/hosts & DNS Configuration

```bash
# /etc/hosts — local DNS (checked before DNS server)
cat /etc/hosts
# Format: IP_address  hostname  alias
# Example:
# 127.0.0.1    localhost
# 192.168.1.10 myserver.local myserver

# DNS servers configuration
cat /etc/resolv.conf            # DNS server settings
# nameserver 8.8.8.8

# /etc/nsswitch.conf — controls lookup order
grep hosts /etc/nsswitch.conf
# hosts: files dns  ← looks in /etc/hosts first, then DNS
```

---

### ✅ Module 9 Checkpoint

```bash
# 1. Find your IP address and gateway
ip addr
ip route

# 2. Test DNS resolution
dig google.com +short
nslookup 8.8.8.8

# 3. Check which ports are listening
ss -tulnp

# 4. Check route to google
traceroute -m 10 google.com
```

---

<a name="module-10"></a>
## MODULE 10 — Shell Scripting

### What You'll Learn
- Writing bash scripts
- Variables, loops, conditions
- Functions
- Practical scripts

---

### 10.1 Your First Script

```bash
#!/bin/bash
# This is a comment
# The first line (#!) is the shebang — tells system which interpreter to use

echo "Hello, World!"
echo "Today is: $(date)"
echo "I am logged in as: $(whoami)"
```

**Save as `hello.sh`, make it executable, run it:**
```bash
chmod +x hello.sh
./hello.sh
# Or: bash hello.sh
```

---

### 10.2 Variables

```bash
#!/bin/bash

# Variables (no spaces around =)
NAME="Alice"
AGE=30
TODAY=$(date +%Y-%m-%d)      # Command substitution

# Use variables with $
echo "Name: $NAME"
echo "Age: $AGE"
echo "Date: $TODAY"
echo "Name: ${NAME}"         # Curly braces — best practice

# Read input from user
read -p "Enter your name: " USERNAME
echo "Hello, $USERNAME!"

# Special variables
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
echo "Last exit code: $?"
echo "Script PID: $$"
```

---

### 10.3 Conditions

```bash
#!/bin/bash

FILE="/etc/passwd"
NUMBER=42

# if-else
if [ -f "$FILE" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi

# if-elif-else
if [ $NUMBER -gt 100 ]; then
    echo "Greater than 100"
elif [ $NUMBER -gt 50 ]; then
    echo "Greater than 50"
else
    echo "50 or less"
fi

# File test operators
[ -f file ]     # Is a regular file
[ -d dir ]      # Is a directory
[ -e path ]     # Exists (file or directory)
[ -r file ]     # Is readable
[ -w file ]     # Is writable
[ -x file ]     # Is executable
[ -s file ]     # File exists and is not empty
[ -z "$var" ]   # String is empty
[ -n "$var" ]   # String is not empty

# Comparison operators
[ "$a" = "$b" ]   # String equal
[ "$a" != "$b" ]  # String not equal
[ $a -eq $b ]     # Numeric equal
[ $a -ne $b ]     # Not equal
[ $a -gt $b ]     # Greater than
[ $a -lt $b ]     # Less than
[ $a -ge $b ]     # Greater than or equal
[ $a -le $b ]     # Less than or equal

# Logical operators
[ condition1 ] && [ condition2 ]   # AND
[ condition1 ] || [ condition2 ]   # OR
```

---

### 10.4 Loops

```bash
#!/bin/bash

# for loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# for loop with range
for i in {1..10}; do
    echo "Count: $i"
done

# for loop with step
for i in {0..20..5}; do   # 0, 5, 10, 15, 20
    echo "$i"
done

# C-style for loop
for ((i=0; i<10; i++)); do
    echo "i = $i"
done

# Loop over files
for file in /etc/*.conf; do
    echo "Config: $file"
done

# while loop
COUNTER=0
while [ $COUNTER -lt 5 ]; do
    echo "Counter: $COUNTER"
    ((COUNTER++))
done

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < /etc/passwd
```

---

### 10.5 Functions

```bash
#!/bin/bash

# Define function
greet() {
    local NAME=$1        # local = only exists inside function
    echo "Hello, $NAME!"
}

check_service() {
    SERVICE=$1
    if systemctl is-active --quiet $SERVICE; then
        echo "$SERVICE is running"
        return 0         # Success
    else
        echo "$SERVICE is NOT running"
        return 1         # Failure
    fi
}

# Call functions
greet "Alice"
greet "Bob"

check_service nginx
if [ $? -eq 0 ]; then
    echo "All good!"
fi
```

---

### 10.6 Practical Scripts

**Disk space alert script:**
```bash
#!/bin/bash
# disk_alert.sh — Alert when disk is over 80%

THRESHOLD=80
HOSTNAME=$(hostname)

df -h | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{print $5, $1}' | while read OUTPUT; do
    USAGE=$(echo $OUTPUT | awk '{print $1}' | cut -d'%' -f1)
    PARTITION=$(echo $OUTPUT | awk '{print $2}')
    
    if [ $USAGE -ge $THRESHOLD ]; then
        echo "ALERT: $HOSTNAME - $PARTITION is at ${USAGE}% capacity!"
        # Could add: mail -s "Disk Alert" admin@example.com <<< "..."
    fi
done
```

**Backup script:**
```bash
#!/bin/bash
# backup.sh — Simple backup with timestamp

SOURCE="/var/www/html"
DEST="/backup"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$DEST/web_backup_$DATE.tar.gz"

mkdir -p $DEST

echo "Starting backup of $SOURCE..."
tar -czf $BACKUP_FILE $SOURCE

if [ $? -eq 0 ]; then
    echo "Backup successful: $BACKUP_FILE"
    echo "Size: $(du -sh $BACKUP_FILE | cut -f1)"
else
    echo "ERROR: Backup failed!"
    exit 1
fi

# Remove backups older than 7 days
find $DEST -name "web_backup_*.tar.gz" -mtime +7 -delete
echo "Old backups cleaned up."
```

---

### ✅ Module 10 Checkpoint

Write a script that:
1. Accepts a username as an argument
2. Checks if the user exists
3. Displays the user's home directory and shell
4. Shows the user's running processes

---

<a name="module-11"></a>
## MODULE 11 — Storage, Disks & Filesystems

### What You'll Learn
- Hard disks and partitions
- Filesystems (ext4, xfs)
- Mounting and unmounting
- LVM (Logical Volume Manager)

---

### 11.1 Disk Basics

```bash
# List disks and partitions
lsblk                   # Tree view of block devices
lsblk -f                # Include filesystem type and UUID
fdisk -l                # Detailed disk info (needs sudo)
sudo fdisk -l /dev/sda  # Specific disk
blkid                   # Block device IDs and filesystems

# Disk device naming:
# /dev/sda  — first SATA/SCSI disk
# /dev/sdb  — second disk
# /dev/sda1 — first partition of sda
# /dev/sda2 — second partition of sda
# /dev/nvme0n1 — NVMe SSD
# /dev/vda  — virtual disk (in VMs)
```

---

### 11.2 Partitioning with fdisk

```bash
sudo fdisk /dev/sdb     # Open partition editor for sdb
# Inside fdisk:
# m — help menu
# p — print partition table
# n — new partition
# d — delete partition
# t — change partition type
# w — write changes and exit
# q — quit without saving
```

---

### 11.3 Creating Filesystems

```bash
# Format partition with ext4 (most common)
sudo mkfs.ext4 /dev/sdb1
sudo mkfs.ext4 -L "mydata" /dev/sdb1    # With label

# Other filesystems:
sudo mkfs.xfs /dev/sdb1          # XFS (better for large files)
sudo mkfs.vfat /dev/sdb1         # FAT32 (USB drives)
sudo mkfs.ntfs /dev/sdb1         # NTFS (Windows compatible)

# Check filesystem
sudo e2fsck -f /dev/sdb1         # Check ext4 filesystem
sudo fsck /dev/sdb1              # Generic filesystem check
```

---

### 11.4 Mounting Filesystems

```bash
# Mount manually (temporary)
sudo mount /dev/sdb1 /mnt/data
sudo mount -t ext4 /dev/sdb1 /mnt/data   # Specify type
sudo mount /dev/sdb1 /media/usb

# Unmount
sudo umount /mnt/data
sudo umount /dev/sdb1            # Same result

# View mounted filesystems
mount                            # All current mounts
df -h                            # Disk usage of mounted filesystems

# Permanent mounting — edit /etc/fstab
sudo vim /etc/fstab

# fstab format:
# device          mountpoint   fstype  options     dump  pass
# /dev/sdb1       /data        ext4    defaults    0     2
# UUID=abc-123    /data        ext4    defaults    0     2   ← use UUID (safer!)

# Get UUID:
sudo blkid /dev/sdb1

# After editing fstab:
sudo mount -a                    # Mount all entries in fstab (test it!)
```

---

### 11.5 LVM — Logical Volume Manager

LVM lets you resize storage without rebooting.

```bash
# LVM concepts:
# PV (Physical Volume) = actual disk/partition
# VG (Volume Group) = pool of storage
# LV (Logical Volume) = usable partition carved from VG

# Create LVM setup:
sudo pvcreate /dev/sdb1          # Create physical volume
sudo vgcreate myvg /dev/sdb1     # Create volume group

sudo lvcreate -L 10G -n mylv myvg         # Create 10GB logical volume
sudo lvcreate -l 100%FREE -n mylv myvg    # Use all remaining space

sudo mkfs.ext4 /dev/myvg/mylv    # Create filesystem on LV
sudo mount /dev/myvg/mylv /data  # Mount it

# Extend a logical volume
sudo lvextend -L +5G /dev/myvg/mylv       # Add 5GB
sudo resize2fs /dev/myvg/mylv             # Resize filesystem after extending

# View LVM info
pvs     # Physical volumes
vgs     # Volume groups
lvs     # Logical volumes
pvdisplay; vgdisplay; lvdisplay           # Detailed info
```

---

### ✅ Module 11 Checkpoint

```bash
# 1. List all block devices
lsblk -f

# 2. Check free space
df -h

# 3. Find largest directories
du -sh /* 2>/dev/null | sort -rh | head -10

# 4. View current fstab
cat /etc/fstab
```

---

<a name="module-12"></a>
## MODULE 12 — Services & Systemd

### What You'll Learn
- What systemd is
- Managing services with systemctl
- Creating your own service
- Viewing journals

---

### 12.1 Understanding systemd

**systemd** is the init system and service manager for modern Linux. It's PID 1 — the first process that starts when Linux boots.

```bash
# Check if you're using systemd
ps -p 1                         # Should show: systemd

# systemd manages:
# - Services (web servers, databases, etc.)
# - Boot process
# - System logs (via journald)
# - Mount points
# - Timers (like cron)
```

---

### 12.2 Managing Services with systemctl

```bash
# Start/stop/restart services
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx     # Reload config (no downtime if supported)

# Enable/disable (starts on boot)
sudo systemctl enable nginx     # Start automatically at boot
sudo systemctl disable nginx    # Don't start at boot
sudo systemctl enable --now nginx   # Enable AND start immediately

# Status
systemctl status nginx          # Detailed status (running? last logs?)
systemctl is-active nginx       # Just active/inactive
systemctl is-enabled nginx      # Just enabled/disabled
systemctl is-failed nginx       # Is it failed?

# List services
systemctl list-units            # All loaded units
systemctl list-units --type=service         # Only services
systemctl list-units --state=failed         # Failed services
systemctl list-unit-files --type=service    # All service files

# System control
sudo systemctl reboot           # Reboot
sudo systemctl poweroff         # Shutdown
sudo systemctl halt             # Halt
sudo systemctl suspend          # Suspend (laptop)
```

---

### 12.3 Creating a Custom Service

```bash
# Create a simple script to run as a service
sudo vim /usr/local/bin/myapp.sh
```

```bash
#!/bin/bash
while true; do
    echo "$(date): Service is running" >> /var/log/myapp.log
    sleep 60
done
```

```bash
sudo chmod +x /usr/local/bin/myapp.sh

# Create the service file
sudo vim /etc/systemd/system/myapp.service
```

```ini
[Unit]
Description=My Custom Application
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/usr/local/bin/myapp.sh
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload        # Reload systemd config
sudo systemctl enable myapp
sudo systemctl start myapp
sudo systemctl status myapp
```

---

### 12.4 Journalctl — Viewing Logs

```bash
# View all logs
journalctl

# View logs for a specific service
journalctl -u nginx
journalctl -u nginx -f          # Follow (like tail -f)
journalctl -u nginx --since today
journalctl -u nginx --since "2025-01-15" --until "2025-01-16"

# System boots
journalctl --list-boots         # List previous boots
journalctl -b                   # Logs from current boot
journalctl -b -1                # Logs from previous boot

# Priority levels
journalctl -p err               # Errors only
journalctl -p warning           # Warnings and above

# Last N lines
journalctl -n 50 -u nginx

# Disk usage of journals
journalctl --disk-usage
journalctl --vacuum-size=500M   # Keep only 500MB of logs
```

---

### ✅ Module 12 Checkpoint

```bash
# 1. Install and manage nginx
sudo apt install nginx
sudo systemctl status nginx
sudo systemctl enable nginx

# 2. View nginx logs
journalctl -u nginx -n 20

# 3. List all failed services
systemctl list-units --state=failed
```

---

<a name="module-13"></a>
## MODULE 13 — Logs & Troubleshooting

### What You'll Learn
- Where log files are
- Reading and analyzing logs
- Common troubleshooting steps

---

### 13.1 Important Log Files

```bash
# Log files location
/var/log/syslog         # General system log (Debian/Ubuntu)
/var/log/messages       # General system log (RHEL/CentOS)
/var/log/auth.log       # Authentication logs (logins, sudo)
/var/log/kern.log       # Kernel messages
/var/log/dmesg          # Boot messages
/var/log/apt/           # Package manager logs (Ubuntu)
/var/log/nginx/         # Nginx access and error logs
/var/log/apache2/       # Apache logs
/var/log/mysql/         # MySQL logs
/var/log/mail.log       # Mail server logs
```

---

### 13.2 Reading Logs

```bash
# Real-time log monitoring
tail -f /var/log/syslog
tail -f /var/log/auth.log
journalctl -f                   # systemd journal in real-time

# Search logs
grep "Failed password" /var/log/auth.log    # Find failed logins
grep "error" /var/log/nginx/error.log
grep -i "oom" /var/log/syslog              # Out of memory errors
grep "segfault" /var/log/syslog

# Multiple files
grep "error" /var/log/*.log

# View boot messages
dmesg                           # Kernel ring buffer
dmesg | grep error
dmesg | grep -i "fail"
dmesg -T                        # Human readable timestamps

# Log rotation
cat /etc/logrotate.conf         # Logrotate config
ls /etc/logrotate.d/            # Per-application rotation configs
sudo logrotate -f /etc/logrotate.conf   # Force rotation
```

---

### 13.3 Troubleshooting Methodology

**When something breaks, follow this process:**

```
1. IDENTIFY the problem clearly
   - What is failing?
   - What error message do you see?
   - When did it start failing?

2. CHECK service status
   systemctl status servicename

3. CHECK logs
   journalctl -u servicename -n 50
   tail -100 /var/log/relevant.log

4. CHECK resources
   df -h          (disk space)
   free -h        (memory)
   top            (CPU usage)

5. CHECK network (if network-related)
   ping gateway
   ss -tulnp      (is the port listening?)

6. CHECK configuration
   nginx -t       (test nginx config)
   apache2 -t     (test apache config)

7. SEARCH for the error message
   Google/documentation

8. APPLY fix, TEST, DOCUMENT
```

---

### ✅ Module 13 Checkpoint

```bash
# 1. View authentication failures
sudo grep "Failed password" /var/log/auth.log | tail -10

# 2. Check for OOM (Out of Memory) events
sudo dmesg | grep -i "oom\|killed"

# 3. See disk usage in /var/log
du -sh /var/log/*  | sort -rh | head -10
```

---

<a name="module-14"></a>
## MODULE 14 — Security & Firewall

### What You'll Learn
- UFW firewall (Ubuntu)
- firewalld (RHEL/CentOS)
- iptables basics
- SSH hardening
- Fail2ban

---

### 14.1 UFW — Uncomplicated Firewall (Ubuntu)

```bash
# Enable/disable UFW
sudo ufw enable
sudo ufw disable
sudo ufw status verbose

# Allow/deny traffic
sudo ufw allow 22                    # Allow SSH (port 22)
sudo ufw allow 80/tcp                # Allow HTTP
sudo ufw allow 443/tcp               # Allow HTTPS
sudo ufw allow from 192.168.1.0/24   # Allow subnet
sudo ufw allow from 192.168.1.100 to any port 22   # Allow IP on port 22
sudo ufw allow 'Nginx Full'          # Allow Nginx profile

sudo ufw deny 23                     # Deny telnet
sudo ufw deny from 1.2.3.4           # Block an IP

# Remove rules
sudo ufw delete allow 80
sudo ufw delete allow 'Nginx HTTP'

# Reset firewall
sudo ufw reset

# View numbered rules (easy deletion)
sudo ufw status numbered
sudo ufw delete 3                    # Delete rule #3
```

---

### 14.2 firewalld (RHEL/CentOS/AlmaLinux)

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --state

# Zones (network trust levels)
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --get-active-zones

# Allow services and ports
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --permanent --add-port=8080/tcp

# Remove
sudo firewall-cmd --permanent --remove-service=http

# Reload after changes
sudo firewall-cmd --reload

# List current rules
sudo firewall-cmd --list-all
```

---

### 14.3 SSH Hardening

```bash
# Edit SSH config
sudo vim /etc/ssh/sshd_config

# Recommended settings:
Port 2222                          # Change default port (optional)
PermitRootLogin no                 # Never allow root login
PasswordAuthentication no          # Disable password auth (use keys!)
PubkeyAuthentication yes           # Allow key-based login
MaxAuthTries 3                     # Max failed attempts before disconnect
LoginGraceTime 20                  # Seconds to complete login
AllowUsers alice bob               # Only allow specific users
ClientAliveInterval 300            # Disconnect idle clients after 5 min
ClientAliveCountMax 2

# After editing, restart SSH:
sudo systemctl restart sshd

# Generate SSH key pair (on your workstation)
ssh-keygen -t ed25519 -C "alice@mycomputer"
# Saves to ~/.ssh/id_ed25519 and ~/.ssh/id_ed25519.pub

# Copy public key to server
ssh-copy-id alice@server_ip
# Or manually:
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

### 14.4 Fail2ban

Fail2ban monitors logs and bans IPs with repeated failures.

```bash
sudo apt install fail2ban

# Configuration (copy default config first)
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo vim /etc/fail2ban/jail.local

# Key settings in jail.local:
[DEFAULT]
bantime = 3600         # Ban for 1 hour
findtime = 600         # Look at last 10 minutes
maxretry = 5           # Ban after 5 failures

[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log

sudo systemctl enable --now fail2ban

# Manage bans
sudo fail2ban-client status
sudo fail2ban-client status sshd            # Status for SSH jail
sudo fail2ban-client set sshd unbanip 1.2.3.4   # Unban an IP
```

---

### ✅ Module 14 Checkpoint

```bash
# 1. Enable UFW and configure basic rules
sudo ufw allow 22
sudo ufw allow 80
sudo ufw enable
sudo ufw status verbose

# 2. View SSH config and understand current settings
sudo sshd -T | grep -E "permitroot|passwordauth|pubkey"

# 3. Check auth log for failed logins
sudo grep "Failed password" /var/log/auth.log | wc -l
```

---

<a name="module-15"></a>
## MODULE 15 — SSH & Remote Administration

### What You'll Learn
- SSH client configuration
- SSH key management
- SCP and rsync file transfer
- SSH tunneling

---

### 15.1 SSH Basics

```bash
# Connect to server
ssh alice@192.168.1.10
ssh alice@192.168.1.10 -p 2222       # Non-standard port
ssh -i ~/.ssh/my_key alice@server    # Specify key file
ssh -v alice@server                  # Verbose (debug connection issues)

# Run command on remote server
ssh alice@server "ls /var/log"
ssh alice@server "sudo systemctl status nginx"

# SSH config file (~/.ssh/config) — saves typing
vim ~/.ssh/config
```

```
Host myserver
    HostName 192.168.1.10
    User alice
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

Host prod
    HostName 10.0.0.1
    User admin
    ProxyJump jumpserver    # Connect through jump/bastion server
```

```bash
# Now you can simply type:
ssh myserver
ssh prod
```

---

### 15.2 File Transfer with SCP and rsync

```bash
# SCP — Secure Copy
scp file.txt alice@server:/home/alice/          # Upload file
scp alice@server:/home/alice/file.txt .         # Download file
scp -r folder/ alice@server:/home/alice/        # Upload directory
scp -P 2222 file.txt alice@server:/tmp/         # Non-standard port

# rsync — efficient synchronization (only transfers changes)
rsync -av file.txt alice@server:/home/alice/         # Upload with verbose
rsync -av folder/ alice@server:/var/www/html/        # Sync directory
rsync -avz --delete folder/ alice@server:/backup/    # Sync, delete remote extras
rsync -avz --exclude '*.log' src/ dest/              # Exclude log files
rsync -avz -e "ssh -p 2222" src/ alice@server:/dest/ # Custom port

# Progress indicator
rsync -avz --progress large_file.iso alice@server:/tmp/
```

---

### 15.3 SSH Tunneling

```bash
# Local port forwarding — access remote service locally
# Forward localhost:8080 to server's port 80
ssh -L 8080:localhost:80 alice@server
# Now browse to http://localhost:8080 — you see the remote web server

# Access database behind firewall:
ssh -L 3306:db_server:3306 alice@bastion_server
# Then: mysql -h 127.0.0.1 -u dbuser -p

# Remote port forwarding — expose local service to remote
ssh -R 9090:localhost:8080 alice@server

# SOCKS5 proxy (route browser through server)
ssh -D 8080 alice@server
# Configure browser to use SOCKS5 proxy localhost:8080
```

---

### ✅ Module 15 Checkpoint

```bash
# 1. Create an SSH config entry for a server
vim ~/.ssh/config

# 2. Generate an ED25519 key pair
ssh-keygen -t ed25519

# 3. Test rsync locally
rsync -av ~/practice/ /tmp/practice_backup/
```

---

<a name="module-16"></a>
## MODULE 16 — Cron Jobs & Automation

### What You'll Learn
- Cron job scheduling
- Crontab syntax
- Anacron and systemd timers

---

### 16.1 Cron — Task Scheduler

```bash
# Edit your crontab
crontab -e              # Edit current user's crontab
crontab -l              # List current user's crontab
crontab -r              # Remove crontab (careful!)
sudo crontab -e -u alice  # Edit alice's crontab

# System-wide cron directories:
/etc/cron.hourly/       # Scripts run every hour
/etc/cron.daily/        # Scripts run every day
/etc/cron.weekly/       # Scripts run every week
/etc/cron.monthly/      # Scripts run every month
/etc/cron.d/            # Custom cron file location
```

---

### 16.2 Crontab Syntax

```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, where 0 and 7 = Sunday)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)

Special characters:
* = every (any value)
, = list (1,3,5 = 1st, 3rd, 5th)
- = range (1-5 = 1st through 5th)
/ = step (*/5 = every 5)

Examples:
0 * * * *       = Every hour (at :00)
0 0 * * *       = Every day at midnight
0 2 * * 0       = Every Sunday at 2 AM
0 2 1 * *       = 1st of every month at 2 AM
*/5 * * * *     = Every 5 minutes
0 9-17 * * 1-5  = Every hour 9 AM-5 PM, Mon-Fri
30 4 1,15 * *   = 4:30 AM on 1st and 15th
```

**Real examples:**
```bash
# Backup web files every night at 2 AM
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1

# Clean temp files every Sunday at 3 AM
0 3 * * 0 find /tmp -type f -mtime +7 -delete

# Check disk space every 30 minutes
*/30 * * * * /usr/local/bin/disk_alert.sh

# Reboot server every Sunday at 4 AM
0 4 * * 0 /sbin/reboot

# Sync files every 15 minutes
*/15 * * * * rsync -az /data/ backup@server:/backup/
```

---

### 16.3 Systemd Timers (Modern Alternative to Cron)

```bash
# Create timer unit
sudo vim /etc/systemd/system/mybackup.timer
```

```ini
[Unit]
Description=Run backup daily

[Timer]
OnCalendar=daily
Persistent=true          # Run if missed (e.g., system was off)

[Install]
WantedBy=timers.target
```

```bash
sudo vim /etc/systemd/system/mybackup.service
```

```ini
[Unit]
Description=My Backup Service

[Service]
ExecStart=/usr/local/bin/backup.sh
```

```bash
sudo systemctl enable mybackup.timer
sudo systemctl start mybackup.timer

# List all timers
systemctl list-timers
```

---

### ✅ Module 16 Checkpoint

```bash
# 1. Add a cron job to log the date every minute (for testing)
crontab -e
# Add: * * * * * date >> /tmp/cron_test.log 2>&1

# Wait 2 minutes, then:
cat /tmp/cron_test.log

# 2. Remove the test cron job
crontab -e  # Delete the line
```

---

<a name="module-17"></a>
## MODULE 17 — Advanced Administration Topics

### What You'll Learn
- Environment variables
- Compression & archives
- System performance tuning
- Kernel parameters (sysctl)
- Regular expressions

---

### 17.1 Environment Variables

```bash
# View all environment variables
env
printenv
printenv PATH           # View specific variable

# Set variables
export MYVAR="hello"                    # Export to child processes
export PATH="$PATH:/usr/local/myapp"    # Add to PATH

# Make permanent — add to:
~/.bashrc                               # Current user, interactive shells
~/.bash_profile or ~/.profile           # Current user, login shells
/etc/environment                        # System-wide
/etc/profile.d/myapp.sh                 # System-wide, modular

# Important variables
echo $HOME              # Home directory
echo $PATH              # Command search path
echo $USER              # Current username
echo $SHELL             # Current shell
echo $HOSTNAME          # Computer hostname
echo $LANG              # Locale
echo $EDITOR            # Default text editor
```

---

### 17.2 Compression & Archives

```bash
# tar — Tape Archive (most common)
tar -czf archive.tar.gz folder/         # Create gzip compressed archive
tar -cjf archive.tar.bz2 folder/        # Create bzip2 compressed archive
tar -cJf archive.tar.xz folder/         # Create xz compressed (smallest)
tar -cf archive.tar folder/             # Create uncompressed archive

tar -xzf archive.tar.gz                 # Extract gzip archive
tar -xzf archive.tar.gz -C /tmp/       # Extract to specific directory
tar -tzf archive.tar.gz                 # List contents without extracting

# Quick reference for tar flags:
# c = create, x = extract, t = list
# z = gzip, j = bzip2, J = xz
# f = filename, v = verbose

# gzip / gunzip
gzip file.txt                    # Compress (creates file.txt.gz, deletes original)
gzip -k file.txt                 # Keep original
gunzip file.txt.gz               # Decompress
gzip -d file.txt.gz              # Same as gunzip

# zip / unzip
zip archive.zip file1.txt file2.txt
zip -r archive.zip folder/
unzip archive.zip
unzip archive.zip -d /tmp/       # Extract to directory
unzip -l archive.zip             # List contents
```

---

### 17.3 Kernel Parameters with sysctl

```bash
# View current kernel parameters
sysctl -a                        # All parameters
sysctl net.ipv4.ip_forward       # Specific parameter
cat /proc/sys/net/ipv4/ip_forward

# Set temporarily (lost on reboot)
sudo sysctl -w net.ipv4.ip_forward=1

# Set permanently — edit /etc/sysctl.conf or /etc/sysctl.d/*.conf
sudo vim /etc/sysctl.d/99-custom.conf
# Add: net.ipv4.ip_forward = 1

sudo sysctl -p                   # Apply changes from /etc/sysctl.conf
sudo sysctl --system             # Apply all sysctl configs

# Common useful parameters:
# net.ipv4.ip_forward=1         — Enable IP routing (for routers/VPNs)
# vm.swappiness=10              — Reduce swap usage (10 = less aggressive)
# net.core.somaxconn=1024       — Increase connection queue
# fs.file-max=65535             — Max open files system-wide
```

---

### 17.4 System Performance

```bash
# Identify bottlenecks
vmstat 2 5              # VM stats every 2 sec, 5 times
iostat -x 2             # Extended I/O stats every 2 sec
sar -u 1 5              # CPU usage (needs sysstat package)
sar -r 1 5              # Memory usage history

# Check open files
lsof                    # List all open files
lsof -u alice           # Files opened by alice
lsof -p 1234            # Files opened by process 1234
lsof -i :80             # What's using port 80

# Limits
ulimit -a               # Current user limits
ulimit -n               # Max open files for current session
ulimit -n 65535         # Increase open files limit (current session)

# Edit limits permanently: /etc/security/limits.conf
sudo vim /etc/security/limits.conf
# alice soft nofile 65535
# alice hard nofile 65535
```

---

### ✅ Module 17 Checkpoint

```bash
# 1. Create a compressed archive of /etc
tar -czf /tmp/etc_backup.tar.gz /etc/

# 2. List what's in the archive
tar -tzf /tmp/etc_backup.tar.gz | head -20

# 3. Check current kernel settings
sysctl vm.swappiness
sysctl net.ipv4.ip_forward

# 4. See your current limits
ulimit -a
```

---

<a name="practice-labs"></a>
## 🔬 PRACTICE LABS & PROJECTS

### Lab 1: Web Server Setup
```bash
# Install and configure nginx
sudo apt install nginx
sudo systemctl enable --now nginx
sudo ufw allow 'Nginx Full'

# Create a custom webpage
sudo vim /var/www/html/index.html

# Check it works
curl http://localhost
```

### Lab 2: User Management Scenario
```bash
# Create team structure:
# Groups: developers, testers, devops
# Users: alice (developers), bob (testers), charlie (devops + sudo)
# Set up shared /srv/project directory accessible to all
sudo groupadd developers
sudo groupadd testers
sudo groupadd devops
# ... complete the scenario yourself
```

### Lab 3: Automated Backup System
Write a complete backup script that:
- Takes backups of /etc and /var/www
- Compresses them with a timestamp
- Removes backups older than 7 days
- Logs all actions
- Sends an alert if disk is over 80%
- Runs via cron at 2 AM daily

### Lab 4: Security Hardening
Harden a fresh Ubuntu server:
- Change SSH port, disable root login, disable password auth
- Enable and configure UFW
- Install fail2ban
- Create a non-root admin user
- Set proper file permissions
- Configure log monitoring

### Lab 5: System Monitoring Script
Write a script that generates a daily report showing:
- Uptime and load
- Memory usage
- Disk usage
- Top 5 CPU processes
- Recent auth failures
- Services that are not running

---

<a name="cheat-sheet"></a>
## 📋 QUICK REFERENCE CHEAT SHEET

### Navigation
```bash
pwd | ls | ls -la | cd /path | cd ~ | cd - | cd ..
```

### File Operations
```bash
touch | mkdir -p | cp -r | mv | rm -rf | find / -name
```

### Permissions
```bash
chmod 755 | chmod +x | chown user:group | chgrp | ls -l
```

### Users
```bash
useradd -m | usermod -aG | userdel -r | passwd | id | whoami
```

### Processes
```bash
ps aux | top | htop | kill PID | kill -9 | pgrep | pkill | jobs
```

### Services
```bash
systemctl start|stop|restart|status|enable|disable service
journalctl -u service -f
```

### Packages (Ubuntu)
```bash
apt update | apt install | apt remove | apt purge | apt autoremove
```

### Networking
```bash
ip addr | ip route | ping | traceroute | ss -tulnp | curl | wget
```

### Disk
```bash
df -h | du -sh | lsblk | mount | umount | fdisk
```

### Logs
```bash
tail -f /var/log/syslog | journalctl -f | dmesg | grep "error" /var/log/*
```

### Compression
```bash
tar -czf archive.tar.gz folder/ | tar -xzf archive.tar.gz | gzip | unzip
```

### SSH
```bash
ssh user@host | ssh-keygen | ssh-copy-id | scp | rsync -avz
```

### Text Processing
```bash
grep -r | sed 's/old/new/g' | awk -F: '{print $1}' | cut -d: -f1 | sort | uniq | wc -l
```

---

## 🗺️ YOUR LEARNING PATH TO LINUX ADMIN

```
BEGINNER          INTERMEDIATE           ADVANCED
─────────         ────────────           ────────
Module 1-4        Module 5-10            Module 11-17
Filesystem        Permissions            LVM
Navigation        Scripting              Security
Basic CLI         Networking             Performance
                  Services               Automation

CERTIFICATIONS TO AIM FOR:
├── LPIC-1 (Linux Professional Institute)
├── CompTIA Linux+
├── RHCSA (Red Hat Certified System Administrator)
└── RHCE (Red Hat Certified Engineer) ← Advanced goal
```

---

## 📚 RECOMMENDED NEXT STEPS

After completing this guide:

1. **Practice daily** — Use Linux as your main OS or spend time in a VM
2. **Build things** — Set up a home lab with real services
3. **Learn containerization** — Docker and Kubernetes (next big step)
4. **Learn configuration management** — Ansible, Puppet, or Chef
5. **Explore cloud** — AWS/GCP/Azure using Linux fundamentals
6. **Pursue certification** — RHCSA or LPIC-1

**Resources:**
- `man` pages — your best friend
- https://linux.die.net — online man pages
- https://www.tldp.org — Linux Documentation Project
- https://overthewire.org — Wargames (learn security hands-on)
- https://linuxupskillchallenge.org — Month-long challenge

---

*Happy learning! Every great Linux administrator started where you are right now. The key is consistent daily practice. 🐧*
