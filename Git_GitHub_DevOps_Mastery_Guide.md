# 🔀 Git & GitHub: DevOps Mastery Guide
### From Absolute Beginner to Expert Engineer

> **Taught by:** Senior DevOps Engineer perspective | 15+ years in production systems
> **Goal:** By the end of this guide, you will confidently use Git/GitHub in real DevOps projects and pass DevOps interviews.

---

## 📚 TABLE OF CONTENTS

**BEGINNER**
1. [What is Version Control?](#1-what-is-version-control)
2. [What is Git?](#2-what-is-git)
3. [Installing Git](#3-installing-git)
4. [Git Architecture](#4-git-architecture)
5. [git init & git clone](#5-git-init--git-clone)
6. [git add, commit, log, status](#6-git-add-commit-log-status)
7. [git diff](#7-git-diff)
8. [.gitignore](#8-gitignore)
9. [git restore & reset basics](#9-git-restore--reset-basics)

**INTERMEDIATE**
10. [Branching](#10-branching)
11. [Merging](#11-merging)
12. [Merge Conflicts](#12-merge-conflicts)
13. [git stash](#13-git-stash)
14. [Git Tags](#14-git-tags)
15. [Git Rebase](#15-git-rebase)
16. [git revert vs reset](#16-git-revert-vs-reset)
17. [Remote Repositories](#17-remote-repositories)
18. [git fetch vs git pull](#18-git-fetch-vs-git-pull)

**GITHUB**
19. [GitHub Repositories](#19-github-repositories)
20. [SSH Keys](#20-ssh-keys)
21. [Fork vs Clone](#21-fork-vs-clone)
22. [Pull Requests](#22-pull-requests)
23. [Code Review Workflow](#23-code-review-workflow)
24. [GitHub Issues & Projects](#24-github-issues--projects)
25. [GitHub Actions Basics](#25-github-actions-basics)

**ADVANCED / DEVOPS LEVEL**
26. [Branching Strategies](#26-branching-strategies)
27. [CI/CD Integration](#27-cicd-integration)
28. [Release Management](#28-release-management)
29. [Git Hooks](#29-git-hooks)
30. [Large Repository Management](#30-large-repository-management)
31. [git bisect](#31-git-bisect)
32. [Cherry-pick](#32-cherry-pick)
33. [Advanced Rebase Workflows](#33-advanced-rebase-workflows)
34. [Monorepo vs Polyrepo](#34-monorepo-vs-polyrepo)
35. [Managing Secrets](#35-managing-secrets)
36. [Git Security Best Practices](#36-git-security-best-practices)

**EXPERT LEVEL**
37. [Git Internals](#37-git-internals)
38. [How Git Stores Objects](#38-how-git-stores-objects)
39. [Packfiles & Performance](#39-packfiles--performance)
40. [Debugging Git Problems](#40-debugging-git-problems)
41. [Handling Production Disasters](#41-handling-production-disasters)

**APPENDIX**
- [Interview Questions & Answers](#interview-questions)
- [Quick Reference Cheat Sheet](#cheat-sheet)
- [Learning Roadmap](#roadmap)

---

# ═══════════════════════════════════
# PART 1: BEGINNER
# ═══════════════════════════════════

---

## 1. What is Version Control?

### Simple Explanation

Imagine you're writing a 50-page report in Microsoft Word. You save it as:
- `report_v1.docx`
- `report_v2.docx`
- `report_final.docx`
- `report_FINAL_REAL.docx`
- `report_FINAL_USE_THIS.docx`

That chaos is what happens **without** version control.

**Version Control System (VCS)** is software that:
- Tracks every change made to files over time
- Remembers WHO changed WHAT and WHEN
- Lets you go back to any previous state
- Allows multiple people to work on the same files simultaneously

### Types of Version Control

| Type | Description | Example |
|------|-------------|---------|
| **Local** | Only on your machine | RCS |
| **Centralized** | One server holds everything | SVN, CVS |
| **Distributed** | Everyone has full copy | **Git**, Mercurial |

Git is **distributed** — every developer has the complete history on their machine. No single point of failure.

### Real-World DevOps Example

```
Without VCS:                          With Git:
────────────                          ──────────
"Who deployed the broken code?"       git log shows: Sarah, 2:45 PM
"I can't undo the mistake!"           git revert restores instantly
"Our files conflict with each other!" git merge handles it gracefully
"What changed in last release?"       git diff v1.0..v2.0
```

### Why VCS is Non-Negotiable in DevOps

- **Audit trail** — compliance requires knowing who changed what
- **Rollback** — production incidents require instant recovery
- **Collaboration** — 50 engineers can't work without it
- **CI/CD** — every pipeline starts with a git event (push, PR)
- **Infrastructure as Code** — Terraform, Kubernetes YAML lives in Git

### 🎯 Practice Task
Think about 3 scenarios in software development where NOT having version control would cause a disaster. Write them down before moving on.

---

## 2. What is Git?

### Simple Explanation

Git is a **free, open-source, distributed version control system** created by **Linus Torvalds** in 2005 (yes, the same person who created Linux). He built it in 2 weeks out of frustration — that's a real story.

**Key properties of Git:**
- **Distributed** — full repo on every machine
- **Fast** — most operations are local, no network needed
- **Non-linear** — branches are cheap and powerful
- **Integrity** — every piece of data is checksummed (SHA-1/SHA-256)
- **Snapshots, not diffs** — Git stores complete snapshots, not change lists

### Git vs Other Tools

| Feature | Git | SVN | CVS |
|---------|-----|-----|-----|
| Distributed | ✅ Yes | ❌ No | ❌ No |
| Offline work | ✅ Yes | ❌ No | ❌ No |
| Branching | ✅ Instant | ⚠️ Slow/expensive | ⚠️ Painful |
| Speed | ✅ Very fast | ⚠️ Slower | ⚠️ Slower |
| Industry use | ✅ Dominant | Declining | Mostly dead |

### Git vs GitHub (Important Distinction!)

This confuses beginners constantly:

```
Git      = The tool (software you install)
GitHub   = A website that hosts Git repositories
           (like Google Drive for Git repos)

Others:  GitLab, Bitbucket, Azure DevOps
         (all Git-based hosting platforms)
```

### 🎯 Practice Task
```bash
# After installing Git (next section), run:
git --version
# You should see something like: git version 2.43.0
```

---

## 3. Installing Git

### Install on Each Platform

**Ubuntu/Debian Linux:**
```bash
sudo apt update
sudo apt install git -y
git --version
```

**CentOS/RHEL/AlmaLinux:**
```bash
sudo dnf install git -y
git --version
```

**macOS:**
```bash
# Option 1: Xcode Command Line Tools
xcode-select --install

# Option 2: Homebrew (recommended)
brew install git
```

**Windows:**
```
Download from: https://git-scm.com/download/win
Use "Git for Windows" — includes Git Bash
```

### First-Time Setup (CRITICAL — Do This Before Anything Else)

```bash
# Set your identity — this gets stamped on EVERY commit you make
git config --global user.name "Your Full Name"
git config --global user.email "you@example.com"

# Set your preferred editor (for commit messages)
git config --global core.editor vim
# Or for nano:
git config --global core.editor nano
# Or for VS Code:
git config --global core.editor "code --wait"

# Set default branch name to 'main' (modern standard)
git config --global init.defaultBranch main

# Enable colored output (makes output readable)
git config --global color.ui auto

# Verify your configuration
git config --list
git config --global --list   # Global settings only
cat ~/.gitconfig             # See the config file directly
```

### Configuration Levels

```bash
# Three levels (each overrides the previous):
git config --system   # /etc/gitconfig — all users on machine
git config --global   # ~/.gitconfig — your user account
git config --local    # .git/config — specific repository only

# Real-world use:
# --global for personal settings (name, email, editor)
# --local for project-specific settings (different email for work repos)
```

### 🎯 Practice Task
```bash
# Set up Git with your details, then verify:
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --list | grep user
```

---

## 4. Git Architecture

### The Three Trees (Most Important Concept!)

This is the mental model that makes EVERYTHING else click. Git has three distinct areas:

```
┌─────────────────────────────────────────────────────────┐
│                    YOUR WORKFLOW                         │
│                                                         │
│  Working         Staging          Repository            │
│  Directory  ──►  Area (Index) ──► (.git folder)         │
│  (your files)    (snapshot prep)  (committed history)   │
│                                                         │
│  git add ─────────────►                                 │
│                   git commit ──────────────►            │
│  ◄──────────────────────────── git checkout/restore     │
└─────────────────────────────────────────────────────────┘
```

**Working Directory** = The files you see and edit on disk. Your actual project files.

**Staging Area (Index)** = A "preparation zone." You choose EXACTLY which changes go into the next commit. This is unique to Git and incredibly powerful.

**Repository (.git folder)** = The database. All your commits, branches, and history live here. Delete this folder = lose all Git history.

### Why the Staging Area Matters

```bash
# Real-world scenario:
# You made 10 changes in 3 files but want two separate commits:
# Commit 1: "Fix login bug" (only changes in auth.py)
# Commit 2: "Update README" (only README.md)

git add auth.py           # Stage only auth.py
git commit -m "Fix login bug"   # Commit just auth.py

git add README.md         # Now stage README
git commit -m "Update README"   # Commit just README
```

### The .git Directory (Don't Touch, But Understand)

```bash
.git/
├── HEAD          ← Points to current branch
├── config        ← Repository-specific config
├── objects/      ← All file contents and commits (the database)
│   ├── pack/     ← Compressed object storage
│   └── ...
├── refs/
│   ├── heads/    ← Local branch pointers
│   └── tags/     ← Tag pointers
├── index         ← Staging area
└── COMMIT_EDITMSG ← Last commit message
```

### Git's SHA-1 Checksums

Every object in Git gets a unique 40-character SHA-1 hash:

```bash
# Example commit hash:
a3f8b2c1d4e5f6789012345678901234567890ab

# This hash is based on:
# - File content
# - Author, date, commit message
# - Parent commit hash
# This makes Git tamper-evident — you can't change history silently
```

### 🎯 Practice Task
Draw the three areas on paper. For each area, write what Git command moves changes INTO it and OUT of it. This diagram will be your mental model for the rest of this guide.

---

## 5. git init & git clone

### git init — Start a New Repository

```bash
# Create a new project from scratch
mkdir my-devops-project
cd my-devops-project
git init

# Output:
# Initialized empty Git repository in /home/you/my-devops-project/.git/

# Verify it worked
ls -la                    # You'll see .git folder
ls -la .git               # See the internal structure

# Initialize in existing directory
cd /existing/project
git init                  # Won't delete your files, just adds .git
```

### git clone — Copy an Existing Repository

```bash
# Clone via HTTPS (easiest, no setup needed)
git clone https://github.com/username/repo.git

# Clone via SSH (recommended for DevOps — no password prompts)
git clone git@github.com:username/repo.git

# Clone to a specific directory name
git clone https://github.com/username/repo.git my-custom-name

# Clone only the latest snapshot (faster, less history — good for CI)
git clone --depth=1 https://github.com/username/repo.git

# Clone a specific branch
git clone -b develop https://github.com/username/repo.git

# Verify what you cloned
cd repo
git log --oneline -5        # See last 5 commits
git remote -v               # See where it came from
```

### Real-World DevOps Example

```bash
# In a CI/CD pipeline (GitHub Actions, Jenkins, etc.)
# This is one of the first things every pipeline does:
git clone --depth=1 git@github.com:myorg/myapp.git
cd myapp
# Now build, test, deploy...

# --depth=1 clones only latest commit
# Saves time and bandwidth in automated pipelines
# This is called a "shallow clone"
```

### 🎯 Practice Task
```bash
# 1. Create a new repo
mkdir git-practice && cd git-practice
git init
ls -la .git

# 2. Clone a real public repo (example: VS Code docs)
cd /tmp
git clone https://github.com/github/gitignore.git
cd gitignore
git log --oneline -5
```

---

## 6. git add, commit, log, status

### The Core Workflow — Your Daily Commands

This is the loop you'll do hundreds of times every day:

```
Edit files → git add → git commit → repeat
```

### git status — Check What's Going On

```bash
git status                    # Full status
git status -s                 # Short/compact format

# Output explanation:
# ?? file.txt       = Untracked (new file Git doesn't know about)
# M  file.txt       = Modified (tracked file with unstaged changes)
# A  file.txt       = Added (staged, ready to commit)
# D  file.txt       = Deleted
# MM file.txt       = Modified in staging AND working directory

# Red = unstaged changes
# Green = staged changes (ready to commit)
```

### git add — Stage Changes

```bash
# Stage a specific file
git add filename.txt

# Stage multiple files
git add file1.txt file2.py file3.js

# Stage all changes in current directory
git add .

# Stage all changes in the whole repo
git add -A
git add --all

# Stage parts of a file (interactive — very powerful!)
git add -p filename.txt
# Git will show each change and ask: stage this? (y/n/s/q)

# Stage all .py files
git add *.py

# Stage a whole directory
git add src/
```

### git commit — Save a Snapshot

```bash
# Commit with message (opens editor if no -m)
git commit -m "Add login feature"

# Add AND commit all tracked files (skip git add for modified files)
git commit -am "Fix typo in README"
# WARNING: -a does NOT add new untracked files

# Write a detailed commit message (editor opens)
git commit

# Amend the last commit (before pushing!)
git commit --amend -m "Corrected commit message"
git commit --amend --no-edit   # Amend without changing message

# Empty commit (useful for triggering CI/CD pipelines)
git commit --allow-empty -m "Trigger CI pipeline"
```

### Writing Good Commit Messages (Professional Standard)

```
Format:
<type>(<scope>): <short summary>

<body — optional, explain WHY not WHAT>

<footer — optional, link issues>

Types:
feat:     New feature
fix:      Bug fix
docs:     Documentation only
style:    Formatting (no logic change)
refactor: Code refactor
test:     Adding tests
chore:    Build process, tooling
ci:       CI/CD changes
perf:     Performance improvement

Examples:
✅ GOOD:
feat(auth): add JWT token refresh mechanism

Previously tokens expired and users were logged out silently.
Now tokens auto-refresh 5 minutes before expiry.

Closes #234

✅ GOOD:
fix(api): handle null response from payment service

✅ GOOD:
chore(deps): bump lodash from 4.17.20 to 4.17.21

❌ BAD:
"fixed stuff"
"WIP"
"asdfgh"
"changes"
"update"
```

### git log — View History

```bash
# Full log
git log

# Compact one-line format
git log --oneline

# Last N commits
git log --oneline -10

# Beautiful graph showing branches
git log --oneline --graph --all

# Detailed log with stats
git log --stat

# Search by author
git log --author="Alice"

# Search by date
git log --since="2025-01-01"
git log --until="2025-06-01"
git log --since="2 weeks ago"

# Search commit messages
git log --grep="login"

# Search by file
git log -- filename.txt

# Show commits that changed a specific string
git log -S "function login"

# Show specific commit
git show a3f8b2c

# Pretty format
git log --pretty=format:"%h %an %ar %s"
# %h = short hash, %an = author name, %ar = relative date, %s = subject
```

### 🎯 Practice Task — Your First Real Workflow

```bash
mkdir my-webapp && cd my-webapp
git init

# Create some files
echo "# My Web App" > README.md
echo "console.log('hello');" > app.js
mkdir src && echo "body { margin: 0; }" > src/style.css

# Check status
git status

# Stage and commit README
git add README.md
git status             # See what changed
git commit -m "docs: add initial README"

# Stage and commit app files
git add app.js src/
git commit -m "feat: add initial application files"

# View your history
git log --oneline
git log --oneline --graph
```

---

## 7. git diff

### What It Does

`git diff` shows exactly what changed — line by line.

```bash
# What changed in working directory (unstaged)
git diff

# What's in staging (staged vs last commit)
git diff --staged
git diff --cached          # Same thing

# Compare two commits
git diff abc123 def456

# Compare two branches
git diff main develop

# Compare with specific commit
git diff HEAD~1            # Compare with 1 commit ago
git diff HEAD~3            # Compare with 3 commits ago

# Compare specific file
git diff -- filename.txt
git diff main develop -- src/app.js

# Show only file names that changed (no content)
git diff --name-only
git diff --name-status     # Also shows M/A/D status

# Word-level diff (easier to read for prose)
git diff --word-diff
```

### Reading git diff Output

```diff
diff --git a/app.js b/app.js
index 3b18e51..7f5a789 100644
--- a/app.js              ← old file (a version)
+++ b/app.js              ← new file (b version)
@@ -1,4 +1,6 @@          ← line numbers: old @-1,4  new @+1,6
 const express = require('express');
-const port = 3000;       ← RED: line removed
+const port = process.env.PORT || 3000;   ← GREEN: line added
+const host = '0.0.0.0';  ← GREEN: new line
 
 app.listen(port);
```

### Real-World DevOps Use

```bash
# Before committing, always review what you're about to commit:
git diff --staged

# In code review, compare feature branch to main:
git diff main..feature/new-login

# In incident response, see what changed between releases:
git diff v1.2.0 v1.3.0
git diff v1.2.0 v1.3.0 -- config/database.yml

# Check if sensitive files changed (security audit):
git diff HEAD~10 -- .env.example docker-compose.yml
```

### 🎯 Practice Task
```bash
# In your my-webapp repo:
echo "version: 2.0" >> README.md
git diff              # See unstaged changes

git add README.md
git diff              # Nothing! (it's staged now)
git diff --staged     # Now you see it
git commit -m "docs: bump version to 2.0"
```

---

## 8. .gitignore

### What It Does

`.gitignore` tells Git which files to completely ignore — never track, never commit.

### Why It's Critical

```
Things you NEVER want in Git:
- Passwords, API keys, secrets (.env files)
- Compiled code (build/, dist/, *.pyc, *.class)
- Dependencies (node_modules/, vendor/)
- IDE settings (.idea/, .vscode/)
- OS files (.DS_Store, Thumbs.db)
- Logs (*.log)
- Temporary files (*.tmp, *.swp)
```

### Creating .gitignore

```bash
# Create at repo root
touch .gitignore
vim .gitignore
```

```gitignore
# ─────────────────────────────────
# Example .gitignore for a Node.js project
# ─────────────────────────────────

# Dependencies (NEVER commit these — huge and reproducible)
node_modules/
npm-debug.log*
yarn-error.log*

# Build output
dist/
build/
.next/
out/

# Environment variables (CRITICAL — contains secrets!)
.env
.env.local
.env.production
.env*.local

# Logs
logs/
*.log

# OS generated files
.DS_Store
.DS_Store?
Thumbs.db
desktop.ini

# IDE/Editor
.idea/
.vscode/
*.swp
*.swo
*~

# Testing
coverage/
.nyc_output/

# Python
__pycache__/
*.py[cod]
*.egg-info/
venv/
.venv/
```

### .gitignore Patterns

```gitignore
# Exact file
secrets.txt

# All files with extension
*.log
*.tmp

# Entire directory
node_modules/
build/

# Files in specific directory
logs/*.log

# Files in ANY subdirectory
**/secrets.txt
**/*.tmp

# Negate (un-ignore something previously ignored)
!important.log

# Ignore everything in a directory except one file
config/*
!config/default.json

# Comments start with #
# Blank lines are ignored
```

### Useful Commands

```bash
# Check if a file is being ignored
git check-ignore -v filename.txt

# See all ignored files
git status --ignored

# Force add an ignored file (sometimes needed)
git add -f forced_file.txt

# Stop tracking a file that's already committed:
git rm --cached .env               # Remove from tracking (file stays on disk)
git commit -m "chore: stop tracking .env"
# Now add .env to .gitignore
```

### Global .gitignore (For OS/IDE Files)

```bash
# Create a global gitignore for YOUR machine
git config --global core.excludesfile ~/.gitignore_global

vim ~/.gitignore_global
# Add OS/IDE specific patterns here:
.DS_Store
.idea/
*.swp
Thumbs.db
```

### Get Pre-Made .gitignore Files

GitHub maintains official .gitignore templates:
```bash
# Download for Node.js project:
curl https://raw.githubusercontent.com/github/gitignore/main/Node.gitignore > .gitignore

# Available for: Python, Java, Go, Ruby, C++, and 150+ others
# Browse at: github.com/github/gitignore
```

### ⚠️ CRITICAL SECURITY WARNING

```bash
# If you accidentally committed secrets:
# 1. The secret is compromised — rotate it IMMEDIATELY
# 2. Removing from Git is not enough if it was pushed!
# 3. Use git-secrets, trufflehog, or gitleaks to scan repos
# 4. Use git filter-branch or BFG Repo Cleaner to rewrite history

# Prevention tools:
pip install pre-commit
npm install -g git-secrets
brew install trufflesecurity/trufflehog/trufflehog
```

### 🎯 Practice Task
```bash
# In your my-webapp project:
echo "DB_PASSWORD=supersecret123" > .env
echo "API_KEY=abc123xyz" >> .env

echo ".env" > .gitignore
echo "node_modules/" >> .gitignore
echo "*.log" >> .gitignore

git add .gitignore
git commit -m "chore: add .gitignore"

git status          # .env should NOT appear
git check-ignore -v .env   # Confirm it's ignored
```

---

## 9. git restore & reset basics

### git restore — Discard Changes

```bash
# Discard changes in working directory (go back to last committed state)
git restore filename.txt

# Discard ALL unstaged changes (careful — can't undo!)
git restore .

# Unstage a file (remove from staging area, keep changes in working dir)
git restore --staged filename.txt

# Restore to a specific commit
git restore --source=HEAD~2 filename.txt
```

### git reset — Move Branch Pointer

```bash
# Soft reset — undo commit, keep changes staged
git reset --soft HEAD~1
# Use case: "I want to redo my last commit message"

# Mixed reset (default) — undo commit, keep changes unstaged
git reset HEAD~1
git reset --mixed HEAD~1
# Use case: "I committed too early, need to redo"

# Hard reset — undo commit AND discard all changes (DESTRUCTIVE!)
git reset --hard HEAD~1
# Use case: "This commit was garbage, throw it all away"
# ⚠️ You LOSE your changes forever with --hard

# Reset to a specific commit
git reset --hard abc1234
```

### Visual Diagram

```
BEFORE:    A → B → C ← HEAD (main)
           (all committed)

git reset --soft HEAD~1:
           A → B ← HEAD (main)
           C's changes are in STAGING AREA ✅

git reset --mixed HEAD~1:
           A → B ← HEAD (main)
           C's changes are in WORKING DIRECTORY ✅

git reset --hard HEAD~1:
           A → B ← HEAD (main)
           C's changes are GONE ❌
```

### When to Use What

| Situation | Command |
|-----------|---------|
| Undo last commit, keep staged | `git reset --soft HEAD~1` |
| Undo last commit, keep as edits | `git reset HEAD~1` |
| Throw away last commit entirely | `git reset --hard HEAD~1` |
| Remove file from staging | `git restore --staged file` |
| Discard edits to a file | `git restore file` |

### 🎯 Practice Task
```bash
# In your repo:
echo "mistake line" >> README.md
git add README.md

git restore --staged README.md     # Unstage it
git status                          # Back to unstaged

git add README.md
git commit -m "mistake commit"

git reset --soft HEAD~1            # Undo commit, keep staged
git status                          # See it's still staged

git reset HEAD~1                   # Undo commit, keep unstaged
git restore README.md              # Discard the change
```

---

### 🏆 BEGINNER CHECKPOINT QUIZ

Test yourself. Try to answer before looking at the answers.

**Q1:** What's the difference between `git add .` and `git add -A`?

**Q2:** You committed sensitive API keys. What's your immediate action?

**Q3:** What does `git diff --staged` show?

**Q4:** What are the three working areas in Git?

**Q5:** What happens if you delete the `.git` folder?

**Q6:** Write a proper commit message for: "I fixed a bug where users couldn't log in when their email had capital letters"

**Q7:** What does `git reset --hard HEAD~1` do? When would you use it?

<details>
<summary>Answers (attempt first!)</summary>

A1: `git add .` adds everything in current directory (including subdirs). `git add -A` adds all changes across the entire repository including deletions.

A2: Rotate the secret IMMEDIATELY (don't just remove from Git). The secret is compromised if it was pushed. Then remove from history using BFG or git filter-branch.

A3: Shows changes that are staged (in index) vs the last commit — what will be in your next commit.

A4: Working Directory, Staging Area (Index), Repository (.git)

A5: You lose ALL Git history. The files remain but you have no version control anymore.

A6: `fix(auth): handle case-insensitive email in login` (with body explaining the bug)

A7: Moves HEAD back one commit AND discards all changes. Use when you want to completely throw away your last commit and its changes.
</details>

---

# ═══════════════════════════════════
# PART 2: INTERMEDIATE
# ═══════════════════════════════════

---

## 10. Branching

### What Are Branches?

A branch is a **lightweight, movable pointer to a commit**. When you create a branch, Git just creates a new pointer — it doesn't copy files. This is why Git branching is near-instant.

```
main:    A → B → C → D
                  ↑
              feature:  E → F
```

Branch `feature` diverges from `C`, has commits `E` and `F`. `main` continues to `D`. They're independent.

### Why Branches Are Essential in DevOps

```
Production workflow (simplified):
─────────────────────────────────
main ──────────────────────────► (production code, always deployable)
  │
  ├─ feature/user-auth ───────►  (engineer 1 working)
  ├─ feature/payment-api ─────►  (engineer 2 working)
  ├─ bugfix/login-crash ──────►  (urgent fix)
  └─ release/v2.1.0 ──────────►  (preparing release)
```

### Working with Branches

```bash
# List branches
git branch               # Local branches
git branch -r            # Remote branches
git branch -a            # All (local + remote)
git branch -v            # With last commit info
git branch --merged      # Branches merged into current
git branch --no-merged   # Branches NOT yet merged

# Create branch
git branch feature/user-login

# Switch to branch
git checkout feature/user-login     # Old way (still works)
git switch feature/user-login       # New way (recommended)

# Create AND switch in one command
git checkout -b feature/user-login  # Old way
git switch -c feature/user-login    # New way (recommended)

# Create branch from specific commit or tag
git switch -c hotfix/v1.2.1 v1.2.0
git switch -c feature/new main

# Rename branch
git branch -m old-name new-name
git branch -M main       # Rename current branch to main

# Delete branch
git branch -d feature/merged-branch    # Safe delete (must be merged)
git branch -D feature/unmerged         # Force delete

# Delete remote branch
git push origin --delete feature/old-branch
```

### HEAD Pointer

`HEAD` is a special pointer that tells Git: "this is where you currently are."

```bash
cat .git/HEAD             # See what HEAD points to
# Output: ref: refs/heads/main

# Detached HEAD — HEAD points to a commit, not a branch
git checkout abc1234      # Now HEAD → commit, not branch
# ⚠️ Commits here are "orphaned" — create a branch to save them!
git switch -c save-my-work   # Create branch to save commits
```

### 🎯 Practice Task
```bash
cd my-webapp

# Create feature branch
git switch -c feature/add-about-page

# Make changes on feature branch
echo "<h1>About Us</h1>" > about.html
git add about.html
git commit -m "feat: add about page"

# Check branches
git branch -v

# Switch back to main
git switch main
ls           # about.html is GONE! (it's on the feature branch)

git switch feature/add-about-page
ls           # about.html is back!
```

---

## 11. Merging

### Types of Merges

**Fast-Forward Merge** — when target branch has no new commits since branch point:
```
Before:     main: A → B
                        ↘
              feature:    C → D

After FF:   main: A → B → C → D
```

**3-Way Merge** — when both branches have diverged:
```
Before:     main: A → B → E
                   ↘
              feature: C → D

After:      main: A → B → E → M (merge commit)
                   ↘         ↗
              feature: C → D
```

### Merging Commands

```bash
# Basic merge
git switch main               # Go to target branch
git merge feature/add-about   # Merge feature into main

# Force a merge commit (even if fast-forward is possible)
git merge --no-ff feature/add-about
# Good for teams — preserves branch history visually

# Squash merge — combine all commits into one
git merge --squash feature/messy-branch
git commit -m "feat: add messy feature (squashed)"
# Good for cleaning up messy feature branch commits

# Abort merge in progress
git merge --abort

# After successful merge, delete the branch
git branch -d feature/add-about
```

### 🎯 Practice Task
```bash
# On main
git switch main
echo "Main change" >> README.md
git commit -am "docs: update README on main"

# Merge feature branch
git merge feature/add-about-page --no-ff
git log --oneline --graph     # See the merge commit
git branch -d feature/add-about-page
```

---

## 12. Merge Conflicts

### Why Conflicts Happen

When two branches modify the **same part of the same file**, Git can't decide which change wins. You must resolve it manually.

```bash
# Conflict markers in file:
<<<<<<< HEAD           ← Your current branch changes
This is from main branch
=======                ← Separator
This is from feature branch
>>>>>>> feature/login  ← Incoming branch changes
```

### Resolving Conflicts Step by Step

```bash
# 1. Attempt merge — Git reports conflicts
git merge feature/login
# OUTPUT: CONFLICT (content): Merge conflict in app.js

# 2. See which files have conflicts
git status
# Shows: both modified: app.js

# 3. Open conflicted file, look for conflict markers
vim app.js
# Find <<<<<<, =======, >>>>>>> markers
# Edit to keep what you want (remove the markers too!)

# 4. After resolving, stage the file
git add app.js

# 5. Complete the merge
git commit   # Git auto-creates merge commit message
# OR: git commit -m "merge: resolve login conflict"

# To abort and start over:
git merge --abort
```

### Conflict Resolution Tools

```bash
# Open visual merge tool
git mergetool                  # Uses configured tool

# Configure a merge tool:
git config --global merge.tool vimdiff
git config --global merge.tool vscode
# For VS Code:
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Popular merge tools:
# vimdiff, meld, kdiff3, p4merge, VS Code
```

### Preventing Conflicts (Best Practices)

```bash
# 1. Keep branches short-lived (merge frequently)
# 2. Always pull/rebase before starting new work
git pull --rebase origin main

# 3. Communicate with team about shared files
# 4. Use feature flags instead of long-lived branches
# 5. Merge main into your feature branch regularly
git switch feature/my-branch
git merge main     # Bring in latest main changes
```

### 🎯 Practice Task — Intentional Conflict

```bash
# Create a conflict:
git switch main
echo "Version: 1.0 (from main)" > version.txt
git add version.txt && git commit -m "chore: add version from main"

git switch -c feature/version
echo "Version: 2.0 (from feature)" > version.txt
git add version.txt && git commit -m "chore: add version from feature"

git switch main
git merge feature/version   # CONFLICT!

# Resolve it, then:
git add version.txt
git commit
git log --oneline --graph
```

---

## 13. git stash

### What Is Stash?

Stash temporarily stores your uncommitted changes so you can switch contexts and come back later.

```
Think of it like: "Save work for later without committing"
```

### Common Stash Scenario

```bash
# You're in the middle of a feature...
# Suddenly: "URGENT! Production is down, fix this bug NOW!"
# But you're not done with your feature — don't commit half-done work

# Solution: stash your work
git stash          # Save current changes
git switch main    # Switch to main for the hotfix
# ... fix the bug, commit, deploy ...
git switch feature/my-feature
git stash pop      # Restore your work
# Continue where you left off
```

### Stash Commands

```bash
# Basic stash
git stash                          # Stash tracked changes
git stash push -m "WIP: user auth" # Stash with a description

# Include untracked files
git stash -u
git stash --include-untracked

# Include ignored files too
git stash -a
git stash --all

# List stashes
git stash list
# Output:
# stash@{0}: WIP on main: abc123 Last commit message
# stash@{1}: WIP: user auth

# Apply stash
git stash pop                      # Apply latest, then DELETE from stash list
git stash apply                    # Apply latest, KEEP in stash list
git stash apply stash@{2}         # Apply specific stash

# See what's in a stash
git stash show stash@{0}
git stash show -p stash@{0}        # Full diff

# Delete stashes
git stash drop stash@{0}          # Delete specific stash
git stash clear                    # Delete ALL stashes

# Create branch from stash (when changes have conflicts)
git stash branch new-branch stash@{0}
```

### 🎯 Practice Task
```bash
echo "Unfinished work..." >> app.js
git stash push -m "WIP: unfinished feature"
git stash list

# Simulate doing other work
git log --oneline

# Restore your work
git stash pop
cat app.js    # Your changes are back
```

---

## 14. Git Tags

### What Are Tags?

Tags are permanent labels for specific commits. Used primarily for **release versions**.

```
Commits:   A → B → C → D → E → F
                       ↑           ↑
                    v1.0.0      v1.1.0
```

### Two Types of Tags

```bash
# Lightweight tag — just a name pointer (not recommended for releases)
git tag v1.0.0

# Annotated tag — full object with author, date, message (RECOMMENDED)
git tag -a v1.0.0 -m "Release version 1.0.0"
git tag -a v1.0.0 -m "Release: First stable release

- User authentication
- Payment processing
- Admin dashboard"

# Tag a specific past commit
git tag -a v0.9.0 abc1234 -m "Beta release"

# List tags
git tag
git tag -l "v1.*"              # List matching pattern
git tag -l --sort=-version:refname   # Sorted newest first

# View tag details
git show v1.0.0

# Push tags to remote (tags are NOT pushed automatically!)
git push origin v1.0.0         # Push one tag
git push origin --tags         # Push all tags
git push --follow-tags         # Push commits AND annotated tags

# Delete tags
git tag -d v1.0.0              # Delete local
git push origin --delete v1.0.0  # Delete remote

# Checkout a tag (to look at code at that version)
git checkout v1.0.0            # Detached HEAD
git switch -c release/v1.0.0   # Create branch from tag
```

### Semantic Versioning (SemVer)

```
v MAJOR . MINOR . PATCH
  1       2       3
  
MAJOR: Breaking changes (1.0.0 → 2.0.0)
MINOR: New features, backward compatible (1.0.0 → 1.1.0)
PATCH: Bug fixes (1.0.0 → 1.0.1)

Pre-release:
v1.0.0-alpha.1
v1.0.0-beta.2
v1.0.0-rc.1     (release candidate)
```

### 🎯 Practice Task
```bash
git tag -a v1.0.0 -m "Initial stable release"
git tag -l
git show v1.0.0
```

---

## 15. Git Rebase

### What is Rebase?

Rebase moves or replays commits from one branch onto another. It gives you a **linear, clean history**.

```
BEFORE rebase:
main:    A → B → C
              ↘
feature:        D → E

AFTER rebase (feature onto main):
main:    A → B → C
                  ↘
feature:            D' → E'  (new commits, same changes)
```

The commits `D` and `E` are **replayed** on top of `C`. They get new SHA hashes (D', E').

### Rebase vs Merge

```
Merge preserves history:        Rebase creates linear history:
────────────────────────        ──────────────────────────────
A - B - E - F (merge commit)    A - B - C - D - E
    \       /
     C - D

Use MERGE when:                 Use REBASE when:
- Public/shared branches        - Local feature branches
- You want to preserve          - Cleaning up before PR
  branch history                - Keeping linear history
- After PR approval             - Interactive cleanup
```

### Basic Rebase

```bash
# Rebase feature branch onto main
git switch feature/my-feature
git rebase main
# This replays your feature commits on top of latest main

# If conflicts during rebase:
# 1. Resolve conflict in file
# 2. git add resolved-file
# 3. git rebase --continue
# Or abort: git rebase --abort
```

### Interactive Rebase — The Power Tool

Interactive rebase lets you rewrite history: squash commits, reorder, edit messages, drop.

```bash
# Interactive rebase for last N commits
git rebase -i HEAD~3       # Edit last 3 commits
git rebase -i HEAD~5       # Edit last 5 commits

# Editor opens with:
pick abc123 First commit
pick def456 Second commit
pick ghi789 Third commit

# Change 'pick' to:
# p, pick   = use commit as-is
# r, reword = use commit but edit message
# e, edit   = use commit but stop to amend
# s, squash = meld into previous commit (keep messages)
# f, fixup  = meld into previous commit (discard message)
# d, drop   = remove commit entirely
# b, break  = stop here (for manual edit)

# Example: squash 3 commits into 1
pick abc123 Add login form
squash def456 Fix login form styling
squash ghi789 Fix login form validation

# Result: one clean commit with combined message
```

### ⚠️ Golden Rule of Rebase

```
NEVER rebase commits that have been pushed to a shared remote branch.

Why: Rebase rewrites history (new SHA hashes).
     If others pulled the old commits, their history diverges.
     This causes chaos and "Why is Git showing 'non-fast-forward'?!" panic.

SAFE to rebase: your local commits not yet pushed
DANGEROUS: rebasing pushed commits others may have
```

### 🎯 Practice Task
```bash
# Create messy commits
git switch -c feature/rebase-practice
echo "line 1" > rebase-test.txt && git add . && git commit -m "add line 1"
echo "line 2" >> rebase-test.txt && git add . && git commit -m "typo fix"
echo "line 3" >> rebase-test.txt && git add . && git commit -m "oops another fix"

# Squash them into one clean commit
git rebase -i HEAD~3
# Change the 2nd and 3rd 'pick' to 'squash'
# Write a good combined message

git log --oneline    # See the clean single commit
```

---

## 16. git revert vs reset

### The Key Decision

```
Are your commits already pushed to remote?
         │
         ├─ NO  → git reset is fine
         │
         └─ YES → ALWAYS use git revert
```

### git revert — Safe Undo (For Pushed Commits)

```bash
# Revert creates a NEW commit that undoes the specified commit
git revert HEAD            # Revert last commit
git revert abc1234         # Revert specific commit
git revert HEAD~2..HEAD    # Revert last 2 commits

# Don't auto-commit (review first)
git revert --no-commit HEAD
# Make changes, then: git commit

# Revert a merge commit
git revert -m 1 abc1234   # -m 1 = keep first parent (main)
```

```
BEFORE:  A → B → C (HEAD, pushed to remote)
             (C has a bug)

AFTER revert:  A → B → C → C' (HEAD)
               (C' undoes C's changes, history preserved)
```

### git reset — Rewrite History (Local Only)

```bash
# Already covered in beginner section, but to recap:
git reset --soft HEAD~1    # Undo commit, keep staged
git reset --mixed HEAD~1   # Undo commit, keep working dir
git reset --hard HEAD~1    # Undo commit, discard changes

# Move to specific commit
git reset --hard abc1234
```

### Interview Classic: "What's the difference?"

```
git revert:
- Creates a new commit that undoes changes
- History is preserved (safe for shared branches)
- Use in production / after pushing

git reset:
- Moves the branch pointer backward
- History is erased (dangerous for shared branches)
- Use for local cleanup before pushing
```

---

## 17. Remote Repositories

### Understanding Remotes

A **remote** is a version of your repository hosted on a server (GitHub, GitLab, etc.).

```bash
# View remotes
git remote                  # List remote names
git remote -v               # With URLs
git remote show origin      # Detailed info

# Add remote
git remote add origin https://github.com/user/repo.git
git remote add upstream https://github.com/original/repo.git

# Change remote URL
git remote set-url origin git@github.com:user/repo.git

# Remove remote
git remote remove upstream

# Rename remote
git remote rename origin main-remote
```

### Push and Pull

```bash
# Push changes to remote
git push origin main           # Push main branch to origin
git push                       # Push current branch (if tracking set)
git push -u origin main        # Push AND set upstream tracking
git push --force-with-lease    # Safer force push (checks remote first)
git push origin --delete branch-name   # Delete remote branch

# ⚠️ git push --force is dangerous on shared branches
# Use --force-with-lease instead

# Pull changes from remote
git pull                       # Fetch + merge
git pull origin main           # Explicit
git pull --rebase              # Fetch + rebase (cleaner history)
git pull --rebase origin main  # Explicit rebase pull
```

### Tracking Branches

```bash
# Set upstream tracking
git branch --set-upstream-to=origin/main main
git push -u origin feature/branch      # Sets upstream when pushing

# View tracking info
git branch -vv

# Pull from tracked upstream
git pull                       # Works if tracking is set
```

---

## 18. git fetch vs git pull

### The Critical Difference

```bash
# git fetch: DOWNLOAD changes but DON'T apply them
git fetch origin
git fetch --all         # Fetch from all remotes

# git pull: DOWNLOAD + APPLY (fetch + merge)
git pull origin main

# Analogy:
# fetch = Download email to your inbox, don't read yet
# pull  = Download AND read the email
```

### Why fetch is Safer

```bash
# Fetch first, review, then merge
git fetch origin
git log HEAD..origin/main --oneline    # What will change?
git diff HEAD..origin/main             # What changed?

# If happy with changes:
git merge origin/main
# OR:
git rebase origin/main

# This gives you control vs pull which just applies immediately
```

### DevOps Best Practice

```bash
# Start of each day:
git fetch origin
git log --oneline HEAD..origin/main   # See what teammates did
git rebase origin/main                # Apply on top of latest

# In CI/CD pipelines, always fetch before build:
git fetch --all --prune               # --prune removes dead remote branches
git checkout main
git reset --hard origin/main          # Force to exact remote state
```

---

### 🏆 INTERMEDIATE CHECKPOINT QUIZ

**Q1:** You have 5 messy commits on a feature branch. Before creating a PR, you want to squash them into 1 clean commit. What command?

**Q2:** You accidentally pushed a broken commit to the shared `main` branch. How do you undo it?

**Q3:** What's the difference between `git merge --ff` and `git merge --no-ff`?

**Q4:** A colleague is working on `feature/login`. You're on `feature/payments`. How do you make sure you have their latest work?

**Q5:** You're in the middle of a bug fix but the build is broken and you need to demo something. What do you do?

**Q6:** When would you use `git fetch` over `git pull`?

---

# ═══════════════════════════════════
# PART 3: GITHUB
# ═══════════════════════════════════

---

## 19. GitHub Repositories

### Creating a Repository

```
1. Go to github.com
2. Click "+" → "New repository"
3. Fill in:
   - Repository name (kebab-case: my-awesome-app)
   - Description
   - Public or Private
   - Initialize with README? (yes for new, no if pushing existing)
   - .gitignore template
   - License
```

### Connecting Local Repo to GitHub

```bash
# If you created empty repo on GitHub:
git init
git add .
git commit -m "feat: initial commit"
git remote add origin git@github.com:username/repo.git
git branch -M main
git push -u origin main

# If you created repo with README on GitHub:
git clone git@github.com:username/repo.git
cd repo
# Start working
```

---

## 20. SSH Keys

### Why SSH for GitHub?

```
HTTPS: Requires username/password (or personal access token) each time
SSH:   One-time setup, then seamless — no credentials needed

In DevOps: CI/CD pipelines use SSH keys, never passwords
```

### Setting Up SSH Keys

```bash
# 1. Generate SSH key pair
ssh-keygen -t ed25519 -C "your@email.com"
# Accept defaults (saves to ~/.ssh/id_ed25519)
# Set a passphrase for security

# Old systems: use RSA
ssh-keygen -t rsa -b 4096 -C "your@email.com"

# 2. Start SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. Copy public key
cat ~/.ssh/id_ed25519.pub
# Copy entire output

# 4. Add to GitHub:
# Profile → Settings → SSH and GPG Keys → New SSH key
# Paste your public key

# 5. Test connection
ssh -T git@github.com
# Should say: "Hi username! You've successfully authenticated..."

# 6. Now clone with SSH:
git clone git@github.com:username/repo.git
```

### Managing Multiple SSH Keys (DevOps Reality)

```bash
# Multiple GitHub accounts or keys? Use ~/.ssh/config
vim ~/.ssh/config
```

```
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work

# Usage:
# git clone git@github-personal:username/repo.git
# git clone git@github-work:company/repo.git
```

---

## 21. Fork vs Clone

```
FORK:
- Creates your own COPY of someone else's repo on GitHub
- On GitHub's servers
- Used for: contributing to open source, customizing a project
- Your fork stays connected to original (can sync)
- Pull Requests go FROM your fork TO the original

CLONE:
- Downloads a repo to your local machine
- Used for: working locally on any repo
- You always clone (your own repos AND forks)

Workflow for Open Source contribution:
1. Fork repo (GitHub button)
2. Clone YOUR fork (to local)
3. Make changes
4. Push to YOUR fork
5. Open Pull Request (your fork → original repo)
```

```bash
# After forking on GitHub:
git clone git@github.com:YOUR_USERNAME/forked-repo.git
cd forked-repo

# Add original as upstream remote
git remote add upstream git@github.com:ORIGINAL_OWNER/repo.git
git remote -v

# Keep your fork synced with original:
git fetch upstream
git merge upstream/main
git push origin main
```

---

## 22. Pull Requests

### What is a Pull Request (PR)?

A Pull Request is a **proposal to merge your branch into another branch**. It's the core code collaboration mechanism on GitHub.

```
Developer's workflow:
1. Create feature branch
2. Make commits
3. Push to remote
4. Open Pull Request: feature → main
5. Team reviews code
6. Changes requested or approved
7. Merge into main
8. Delete feature branch
```

### Creating a PR

```bash
# Push your feature branch
git push -u origin feature/add-dark-mode

# On GitHub:
# 1. "Compare & pull request" notification appears
# 2. Or: Pull Requests tab → "New pull request"
# 3. Set base: main, compare: feature/add-dark-mode
# 4. Write PR description:
```

**Great PR description template:**
```markdown
## What does this PR do?
Adds dark mode toggle to the settings page.

## Why?
Users requested dark mode in #issue-45

## How to test?
1. Go to /settings
2. Toggle "Dark Mode"
3. Verify all pages respect the setting

## Screenshots
[screenshot here]

## Checklist
- [x] Tests added/updated
- [x] Documentation updated
- [x] No console errors
- [x] Works on mobile

Closes #45
```

### PR Review Process

```bash
# Review someone's PR locally
git fetch origin
git switch -c review/feature origin/feature/add-dark-mode

# Test the code
# Leave review comments on GitHub
# Approve or Request Changes
```

---

## 23. Code Review Workflow

### The Professional Code Review Process

```
1. PR created → auto-assigned reviewers
2. CI/CD runs tests automatically
3. Reviewers read code, leave comments
4. Author responds and makes changes
5. Reviewers re-review and approve
6. At least 1-2 approvals required to merge
7. Merge + delete branch + close linked issue
```

### GitHub Branch Protection Rules (DevOps Must-Know)

```
Settings → Branches → Add Rule:
- Require pull request reviews before merging (min 1-2)
- Require status checks to pass (CI must pass)
- Require branches to be up to date
- Include administrators
- Restrict who can push to this branch
```

### Review Comment Types

```
Comment:          General discussion
Suggestion:       Propose specific code change (author can accept 1-click)
Request changes:  Blocks merge until resolved
Approve:          LGTM (looks good to me)
```

---

## 24. GitHub Issues & Projects

### GitHub Issues

```
Issues track:
- Bugs (label: bug)
- Features (label: enhancement)
- Tasks (label: task)
- Questions (label: question)

Linking issues to PRs:
In PR description: "Closes #42" or "Fixes #42"
When PR merges, issue auto-closes
```

### Issue Templates

```markdown
<!-- .github/ISSUE_TEMPLATE/bug_report.md -->
## Bug Description
What happened?

## Expected Behavior
What should happen?

## Steps to Reproduce
1. Go to...
2. Click on...

## Environment
- OS: Ubuntu 22.04
- Browser: Chrome 120
- Version: v1.2.3
```

### GitHub Projects

```
GitHub Projects = Kanban board + Roadmap for your issues/PRs

Views:
- Board (Kanban: Todo → In Progress → Done)
- Table (spreadsheet view)
- Roadmap (timeline view)

Great for: sprint planning, release tracking, team coordination
```

---

## 25. GitHub Actions Basics

### What is GitHub Actions?

GitHub Actions is a **CI/CD platform built into GitHub**. It automatically runs workflows when events happen (push, PR, schedule, etc.).

```
Event (push to main)
  → Trigger Workflow
    → Job runs on GitHub server
      → Steps execute
        → Build, Test, Deploy
```

### Your First Workflow

```yaml
# .github/workflows/ci.yml

name: CI Pipeline                    # Workflow name

on:                                  # Trigger events
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:                              # Job name
    runs-on: ubuntu-latest           # Runner (GitHub-hosted VM)
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4      # Built-in action
    
    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '20'
    
    - name: Install dependencies
      run: npm ci                    # Run shell command
    
    - name: Run tests
      run: npm test
    
    - name: Build
      run: npm run build
```

### Common Workflow Patterns

```yaml
# Deploy to server after tests pass
deploy:
  needs: test                        # Runs AFTER 'test' job
  runs-on: ubuntu-latest
  if: github.ref == 'refs/heads/main'  # Only on main branch
  
  steps:
  - uses: actions/checkout@v4
  - name: Deploy
    run: |
      echo "$SSH_KEY" > deploy_key
      chmod 600 deploy_key
      ssh -i deploy_key user@server "cd /app && git pull && pm2 restart app"
    env:
      SSH_KEY: ${{ secrets.DEPLOY_SSH_KEY }}   # Use GitHub Secrets
```

### GitHub Secrets

```
Settings → Secrets and variables → Actions → New repository secret

NEVER put secrets in workflow files!
Always use: ${{ secrets.MY_SECRET }}
```

---

# ═══════════════════════════════════
# PART 4: ADVANCED / DEVOPS LEVEL
# ═══════════════════════════════════

---

## 26. Branching Strategies

### GitFlow

Best for: projects with scheduled releases, multiple versions in production

```
main ──────────────────────────────────────► (production)
  │                                    ↑
  └─► develop ────────────────────────┤   (integration)
         │               ↑            │
         ├─ feature/* ───┘            │
         │                            │
         └─ release/1.2 ──────────────┘
                  │
                  └─ hotfix/* → main + develop
```

```bash
# GitFlow branch types:
# main/master   — production, only merge from release/* or hotfix/*
# develop       — integration branch, feature branches merge here
# feature/*     — new features (from develop, back to develop)
# release/*     — release prep (from develop, to main + develop)
# hotfix/*      — urgent production fixes (from main, to main + develop)

# With git-flow tool:
brew install git-flow
git flow init

git flow feature start user-auth
git flow feature finish user-auth

git flow release start 1.2.0
git flow release finish 1.2.0

git flow hotfix start fix-payment-crash
git flow hotfix finish fix-payment-crash
```

### Trunk Based Development (TBD)

Best for: high-frequency releases, CI/CD-heavy teams, modern DevOps

```
main ──────────────────────────────────────► (trunk, always deployable)
      │       │       │       │
    short   short   short   short
    feature feature feature feature
    (<1day) (<1day) (<1day) (<1day)
```

```bash
# Principles:
# - Only ONE long-lived branch: main/trunk
# - Feature branches live max 1-2 days
# - All engineers commit to trunk multiple times per day
# - Feature flags hide incomplete features
# - Comprehensive automated tests

# Feature flag example in code:
if FEATURE_FLAGS['new_payment_ui']:
    render_new_payment()
else:
    render_old_payment()
```

### Comparison

| Aspect | GitFlow | Trunk Based |
|--------|---------|-------------|
| Release frequency | Weekly/monthly | Daily/hourly |
| Branch count | Many | Few |
| Complexity | Higher | Lower |
| CI/CD integration | Moderate | Excellent |
| Merge conflicts | More | Fewer |
| Used by | Enterprise | Google, Facebook, Netflix |

---

## 27. CI/CD Integration

### Full GitHub Actions Pipeline

```yaml
# .github/workflows/full-pipeline.yml
name: Full CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # ─── Stage 1: Code Quality ───
  lint:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with: { node-version: '20' }
    - run: npm ci
    - run: npm run lint
    - run: npm run type-check

  # ─── Stage 2: Testing ───
  test:
    runs-on: ubuntu-latest
    needs: lint
    services:
      postgres:                          # Spin up test database
        image: postgres:15
        env:
          POSTGRES_PASSWORD: testpass
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with: { node-version: '20' }
    - run: npm ci
    - run: npm test
      env:
        DATABASE_URL: postgresql://postgres:testpass@localhost/test

  # ─── Stage 3: Build Docker Image ───
  build:
    runs-on: ubuntu-latest
    needs: test
    if: github.ref == 'refs/heads/main'
    outputs:
      image-tag: ${{ steps.meta.outputs.tags }}
    steps:
    - uses: actions/checkout@v4
    - name: Login to registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
    - name: Build and push
      uses: docker/build-push-action@v5
      with:
        push: true
        tags: ${{ steps.meta.outputs.tags }}

  # ─── Stage 4: Deploy to Staging ───
  deploy-staging:
    runs-on: ubuntu-latest
    needs: build
    environment: staging                 # Requires approval if configured
    steps:
    - name: Deploy to staging
      run: |
        curl -X POST ${{ secrets.DEPLOY_WEBHOOK_STAGING }} \
          -H "Authorization: Bearer ${{ secrets.DEPLOY_TOKEN }}" \
          -d '{"image": "${{ needs.build.outputs.image-tag }}"}'

  # ─── Stage 5: Deploy to Production ───
  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    environment: production             # Requires manual approval!
    steps:
    - name: Deploy to production
      run: |
        # Your deployment script
        echo "Deploying to production..."
```

---

## 28. Release Management

```bash
# Semantic versioning automation with conventional commits

# Tools for auto-versioning:
# semantic-release, release-please, standard-version

# semantic-release workflow:
npm install -g semantic-release

# It reads commit messages to determine version bump:
# feat: → minor bump (1.0.0 → 1.1.0)
# fix:  → patch bump (1.0.0 → 1.0.1)
# BREAKING CHANGE: → major bump (1.0.0 → 2.0.0)

# GitHub Release creation via Actions:
- name: Create Release
  uses: actions/create-release@v1
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  with:
    tag_name: v${{ steps.version.outputs.version }}
    release_name: Release v${{ steps.version.outputs.version }}
    body: ${{ steps.changelog.outputs.changelog }}
    draft: false
    prerelease: false
```

---

## 29. Git Hooks

### What Are Hooks?

Hooks are scripts that run automatically at specific Git events. They live in `.git/hooks/`.

```bash
ls .git/hooks/
# pre-commit, pre-push, commit-msg, post-merge, etc.
```

### Useful Hooks

```bash
# pre-commit hook — run before commit is created
vim .git/hooks/pre-commit
```

```bash
#!/bin/bash
# Run linter before allowing commit
echo "Running linter..."
npm run lint
if [ $? -ne 0 ]; then
    echo "❌ Linting failed! Fix errors before committing."
    exit 1
fi

# Run tests
echo "Running tests..."
npm test
if [ $? -ne 0 ]; then
    echo "❌ Tests failed! Fix them before committing."
    exit 1
fi

echo "✅ All checks passed!"
exit 0
```

```bash
chmod +x .git/hooks/pre-commit
```

```bash
# commit-msg hook — validate commit message format
vim .git/hooks/commit-msg
```

```bash
#!/bin/bash
MSG=$(cat $1)
PATTERN="^(feat|fix|docs|style|refactor|test|chore|ci|perf)(\(.+\))?: .{3,}"

if ! echo "$MSG" | grep -qE "$PATTERN"; then
    echo "❌ Invalid commit message format!"
    echo "Use: type(scope): description"
    echo "Example: feat(auth): add JWT refresh"
    exit 1
fi
exit 0
```

### Sharing Hooks (pre-commit Framework)

```bash
# .git/hooks are NOT committed to repo
# Use pre-commit framework to share hooks:
pip install pre-commit

# .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
    - id: trailing-whitespace
    - id: end-of-file-fixer
    - id: check-yaml
    - id: detect-private-key          # Catch accidental secrets!
    
  - repo: https://github.com/psf/black
    rev: 23.11.0
    hooks:
    - id: black                       # Python formatter

# Install hooks
pre-commit install

# Run manually
pre-commit run --all-files
```

---

## 30. Large Repository Management

```bash
# Shallow clones (CI/CD optimization)
git clone --depth=1 repo.git           # Only latest commit
git clone --depth=10 repo.git          # Last 10 commits
git fetch --depth=1                    # Shallow fetch

# Partial clone — skip large files
git clone --filter=blob:none repo.git  # Skip file content (fetch on demand)
git clone --filter=tree:0 repo.git     # Skip trees too

# Git LFS (Large File Storage) — for binaries, assets, ML models
git lfs install
git lfs track "*.psd"
git lfs track "*.mp4"
git lfs track "models/*.bin"

# .gitattributes (created by git lfs track):
# *.psd filter=lfs diff=lfs merge=lfs -text

git add .gitattributes
git add large_file.psd
git commit -m "chore: add design file via LFS"

# Sparse checkout — only checkout specific directories
git sparse-checkout init
git sparse-checkout set src/ docs/
# Only checks out src/ and docs/ — useful in monorepos
```

---

## 31. git bisect

### Binary Search for Bugs

`git bisect` finds the exact commit that introduced a bug using binary search. Incredibly powerful for debugging.

```bash
# You know:
# - Latest commit: broken
# - v1.0.0 tag: working
# - Somewhere between them, a commit broke it

git bisect start
git bisect bad                    # Current commit is broken
git bisect good v1.0.0           # v1.0.0 was working

# Git checks out a commit in the middle
# You test: is this broken?

# If broken:
git bisect bad

# If working:
git bisect good

# Git narrows down... after ~10 iterations:
# "abc1234 is the first bad commit"

git bisect reset     # Return to HEAD when done

# Automate with a test script:
git bisect start
git bisect bad
git bisect good v1.0.0
git bisect run npm test    # Auto-test each commit
# Git runs your test on each candidate — finds the culprit automatically
```

---

## 32. Cherry-pick

### What It Does

Cherry-pick applies a specific commit from one branch to another.

```
main:    A → B → C → D → E
              ↑
feature:      F → G → H

# You want commit G's fix on main WITHOUT merging the whole branch:
git cherry-pick G

main:    A → B → C → D → E → G'
```

```bash
# Cherry-pick a single commit
git cherry-pick abc1234

# Cherry-pick a range of commits
git cherry-pick abc123..def456     # Commits AFTER abc123 up to def456
git cherry-pick abc123^..def456    # Including abc123

# Cherry-pick without committing (review first)
git cherry-pick --no-commit abc1234
git diff                            # Review
git commit                          # Then commit

# If conflict:
# Resolve conflict
git add file
git cherry-pick --continue
# Or abort:
git cherry-pick --abort
```

### Real-World Use Case

```bash
# Production is on 'main' at v1.5.0
# Develop is much further ahead (v2.0 features)
# Urgent security fix is on develop at commit abc1234

# Cherry-pick JUST the security fix to main:
git switch main
git cherry-pick abc1234
git tag -a v1.5.1 -m "Security patch"
git push origin main --tags
```

---

## 33. Advanced Rebase Workflows

```bash
# Rebase onto — move branch to a different base
git rebase --onto main feature/base feature/child
# Moves feature/child from being based on feature/base to main

# Autosquash — auto-squash fixup commits
git commit --fixup abc1234         # Creates a "fixup!" commit
git rebase -i --autosquash HEAD~5  # Auto-squashes them in

# Preserve merges during rebase
git rebase --rebase-merges main

# Interactive rebase operations:
# Exec — run shell command after a commit:
pick abc123 My commit
exec npm test          # Run tests after this commit
pick def456 Next commit
```

---

## 34. Monorepo vs Polyrepo

### Monorepo

All projects in one repository:

```
my-company/
├── services/
│   ├── api/
│   ├── auth/
│   └── payments/
├── packages/
│   ├── ui-components/
│   └── utils/
├── apps/
│   ├── web/
│   └── mobile/
└── tools/
```

```bash
# Tools for monorepo management:
# Nx, Turborepo, Bazel, Lerna

# Sparse checkout for large monorepos
git sparse-checkout set services/api packages/utils

# Only build what changed
# Nx: nx affected:build
# Turborepo: turbo run build --filter="...[origin/main]"
```

| | Monorepo | Polyrepo |
|--|---------|---------|
| Code sharing | Easy | Complex |
| Dependency management | Unified | Independent |
| CI/CD complexity | Higher | Lower per repo |
| Used by | Google, Meta, Microsoft | Most teams |
| Tooling | Specialized needed | Standard Git |

---

## 35. Managing Secrets

### The Problem

```bash
# Secrets that MUST NEVER go in Git:
# - Passwords, API keys, tokens
# - Database credentials
# - SSL certificates / private keys
# - AWS/GCP/Azure credentials
# - SSH private keys
```

### Prevention

```bash
# 1. Scanning tools (install on all developer machines)
# trufflehog
trufflehog git file://./  --only-verified

# gitleaks
gitleaks detect --source .

# 2. Pre-commit hook to scan
pip install pre-commit
# Add detect-secrets or gitleaks to .pre-commit-config.yaml

# 3. Never commit .env files — use .env.example instead
echo ".env" >> .gitignore
cp .env .env.example
# Remove actual values from .env.example
git add .env.example .gitignore
```

### Removing Secrets from History

```bash
# If secret was committed — ROTATE IT FIRST!

# Using BFG Repo Cleaner (faster than filter-branch)
java -jar bfg.jar --replace-text passwords.txt
java -jar bfg.jar --delete-files id_rsa

# Using git filter-repo (modern replacement)
pip install git-filter-repo
git filter-repo --path secret.txt --invert-paths
git filter-repo --replace-text <(echo "API_KEY=real_key==>API_KEY=REDACTED")

# Force push cleaned history
git push --force --all
git push --force --tags

# Notify ALL team members to re-clone — their local copies still have the secret
```

### Secret Management Tools

```
Development: .env files (in .gitignore)
CI/CD:       GitHub Secrets, GitLab CI Variables
Production:  HashiCorp Vault, AWS Secrets Manager,
             GCP Secret Manager, Azure Key Vault
Kubernetes:  Kubernetes Secrets (base64 encoded — not true encryption)
             Better: Sealed Secrets, External Secrets Operator
```

---

## 36. Git Security Best Practices

```bash
# 1. Sign commits with GPG (prove it's really you)
gpg --full-generate-key
gpg --list-secret-keys --keyid-format=long

git config --global user.signingkey YOUR_KEY_ID
git config --global commit.gpgsign true

git commit -S -m "Signed commit"   # -S = sign
git log --show-signature -1

# Add GPG key to GitHub:
# Settings → SSH and GPG Keys → New GPG key

# 2. Verify signatures
git verify-commit abc1234
git log --show-signature

# 3. HTTPS credential security
git config --global credential.helper store   # Stores in plaintext (OK for servers)
git config --global credential.helper cache   # In-memory (temporary)
# Use SSH instead of HTTPS for better security

# 4. Branch protection (GitHub settings):
# - Require signed commits
# - Require PR reviews
# - Require status checks
# - Restrict force push

# 5. Audit log
git log --all --full-history -- sensitive-file.txt
```

---

### 🏆 ADVANCED CHECKPOINT QUIZ

**Q1:** Your company uses GitFlow. A critical bug is in production (on `main`). Walk through the exact branching strategy to fix it.

**Q2:** A developer force-pushed to main and broke the branch for the whole team. How do you recover?

**Q3:** You have 8 commits on a feature branch. 4 are real commits, 4 are "fix typo" commits. How do you clean this up before merging?

**Q4:** Describe the security risk of using `git reset --hard` on a pushed branch and how to avoid it.

**Q5:** How would you find which commit introduced a performance regression from 6 months ago?

---

# ═══════════════════════════════════
# PART 5: EXPERT LEVEL
# ═══════════════════════════════════

---

## 37. Git Internals

### Everything is an Object

Git stores 4 types of objects in `.git/objects/`:

| Object | What it stores |
|--------|---------------|
| **blob** | File content |
| **tree** | Directory listing (files + subdirs) |
| **commit** | Commit metadata + pointer to tree |
| **tag** | Annotated tag object |

```bash
# See the objects:
find .git/objects -type f

# Examine object types and content:
git cat-file -t abc123         # Type of object
git cat-file -p abc123         # Content of object
git cat-file -s abc123         # Size of object
```

### How a Commit Works

```bash
# 1. You run: git add file.txt
# Git creates a blob object with file content:
git hash-object -w file.txt
# Output: a8c3f4d... (SHA of file content)

# 2. You run: git commit
# Git creates a tree object (directory snapshot):
# tree a8c3f4d README.md
# tree b7d2e1a src/

# Git creates a commit object:
# tree: (SHA of root tree)
# parent: (SHA of previous commit)
# author: Alice <alice@example.com>
# committer: Alice <alice@example.com>
# message: feat: add login

# 3. Branch pointer (refs/heads/main) moves to new commit SHA
```

### Exploring Internals

```bash
# Create a file and trace Git's storage:
echo "Hello Git!" > test.txt
git add test.txt

# Find the blob object:
git ls-files --stage test.txt
# Output: 100644 8ab686e... 0 test.txt

# See blob content:
git cat-file -p 8ab686e
# Output: Hello Git!

# After commit, inspect the commit object:
git cat-file -p HEAD
# tree abc123...
# parent def456...
# author ...

# Inspect the tree:
git cat-file -p HEAD^{tree}
# 100644 blob 8ab686e test.txt

# Draw the full picture manually!
```

---

## 38. How Git Stores Objects

### Content-Addressable Storage

```bash
# Git uses SHA-1 (moving to SHA-256) to address objects
# Object address = hash of CONTENT
# Same content → same hash (deduplication!)

# This means:
# - Two repos with same file content share the same blob hash
# - Renaming a file = new tree object, same blob object
# - Git KNOWS if content hasn't changed

# Computing SHA manually:
echo -n "blob 12\0Hello, Git!" | sha1sum
# This is exactly how Git hashes content

# Low-level plumbing commands:
git hash-object test.txt                    # Get SHA without storing
git hash-object -w test.txt                 # Compute and STORE
echo "test content" | git hash-object -w --stdin  # Hash stdin
```

### The Ref System

```bash
# Branches are just files containing a commit SHA
cat .git/refs/heads/main        # Contains: abc1234...

# Tags
cat .git/refs/tags/v1.0.0      # Points to commit or tag object

# HEAD
cat .git/HEAD                   # refs/heads/main (symbolic ref)
                                # OR a commit SHA (detached HEAD)

# Remote tracking refs
cat .git/refs/remotes/origin/main   # Last known state of remote

# Packed refs (optimization for many refs)
cat .git/packed-refs
```

---

## 39. Packfiles & Performance

### How Objects Get Packed

```bash
# Individual objects are inefficient for many files
# Git packs them into packfiles for efficiency

ls .git/objects/pack/
# .pack = binary packed objects
# .idx  = index for quick lookup

# Trigger repacking manually:
git gc                           # Garbage collection + repack
git gc --aggressive              # More thorough (takes longer)

# View pack stats:
git count-objects -v
git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -10
# Shows largest objects in pack

# Find large files in history:
git rev-list --objects --all | \
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | \
  sed -n 's/^blob //p' | sort -k2 -rn | head -20

# Remove large files from history:
git filter-repo --strip-blobs-bigger-than 1M
```

---

## 40. Debugging Git Problems

### Common Problems & Solutions

```bash
# ─── "Repository seems to be broken" ───
git fsck                          # Check integrity
git fsck --full                   # Full check
git fsck --unreachable            # Find orphaned objects

# ─── Recover lost commits ───
git reflog                        # The lifesaver!
# Shows EVERY HEAD movement in the last 90 days
git reflog show main
git checkout abc1234              # Restore lost commit
git switch -c recovered-work      # Save it to a branch

# ─── "Detached HEAD" recovery ───
git reflog                        # Find last good position
git switch main                   # OR
git switch -c new-branch          # Save detached work

# ─── Corrupted repository ───
git fsck --full > /tmp/fsck.log
cat /tmp/fsck.log
# Common: "dangling commit" = orphaned, not in any branch (safe to ignore usually)
# "missing blob" = actual corruption, restore from remote

# Fix corrupted objects:
git remote add recovery git@github.com:user/repo.git
git fetch recovery
git fsck

# ─── Wrong author on commits ───
# Fix last commit:
git commit --amend --author="Alice <alice@example.com>"

# Fix multiple commits (use filter-repo):
git filter-repo --email-callback '
  return b"alice@correct.com" if email == b"alice@wrong.com" else email
'

# ─── Accidentally deleted branch ───
git reflog | grep "branch-name"   # Find last commit on branch
git switch -c recovered abc1234   # Recreate from SHA

# ─── Merge went wrong, want to start over ───
git merge --abort                 # If still in progress
git reset --hard ORIG_HEAD        # After completed merge!

# ─── Rebase went wrong ───
git rebase --abort
# Or recover with:
git reflog                        # Find pre-rebase state
git reset --hard HEAD@{5}         # Go back to before rebase
```

---

## 41. Handling Production Disasters

### Scenario 1: Wrong Code Merged to Main (Pre-Deploy)

```bash
# Option 1: Revert the merge commit
git revert -m 1 MERGE_COMMIT_SHA
git push origin main

# Option 2: Reset (if no one else has pulled)
git reset --hard PRE_MERGE_SHA
git push --force-with-lease origin main
```

### Scenario 2: Broken Deploy in Production

```bash
# Immediate rollback workflow:
# 1. Check what's deployed
git log origin/main --oneline -5

# 2. Identify last good commit
git log --oneline  # Find it

# 3a. Revert to last working tag
git revert v1.5.0..HEAD      # Revert all commits since v1.5.0
git tag -a v1.5.1 -m "Emergency rollback"
git push origin main --tags
# Trigger deploy

# 3b. Or: Deploy previous Docker image via CI/CD
# Tag the previous image as latest and redeploy
```

### Scenario 3: Developer Pushed Secrets to Public Repo

```bash
# IMMEDIATE ACTIONS (first 5 minutes):

# 1. ROTATE THE SECRET NOW (before everything else)
#    - AWS: IAM → Create new key, delete old
#    - GitHub token: Settings → Tokens → Revoke
#    - DB password: ALTER USER password = 'newpassword'

# 2. Make repo private immediately (if public)
#    GitHub → Settings → Danger Zone → Make private

# 3. Remove from Git history
git filter-repo --replace-text <(echo "${SECRET}==>REDACTED")
git push --force --all
git push --force --tags

# 4. Notify all devs to re-clone

# 5. Post-mortem and add detection hooks
```

### Scenario 4: Diverged History After Force Push

```bash
# Developer force-pushed, now your local is behind
# DON'T do git pull — it will create a mess

# Safe approach:
git fetch origin
git log --oneline HEAD...origin/main  # See divergence

# If you have no local commits to preserve:
git reset --hard origin/main

# If you have local commits to preserve:
git rebase origin/main              # Replay your commits on new base
```

### The Reflog — Your Safety Net

```bash
# Git keeps EVERY action for 90 days in the reflog
# You can recover from almost anything!

git reflog                           # Full history
git reflog show HEAD                 # HEAD movements
git reflog show main                 # Branch movements
git reflog --all                     # All refs

# Time-based reflog:
git reflog show --date=local         # Human readable timestamps
git checkout "main@{3 days ago}"     # Where was main 3 days ago?
git checkout "HEAD@{30 minutes ago}" # Undo last 30 minutes

# Recovery workflow:
git reflog                           # Find the good SHA
git reset --hard abc1234             # Go back there
```

---

# ═══════════════════════════════════
# APPENDIX
# ═══════════════════════════════════

---

<a name="interview-questions"></a>
## 🎤 DevOps Interview Questions

### Entry Level

**Q: What is Git and why is it used?**
A: Git is a distributed version control system. It tracks changes to files over time, allows multiple developers to collaborate, provides full history and rollback capability, and is the foundation of modern CI/CD pipelines.

**Q: What's the difference between git pull and git fetch?**
A: `git fetch` downloads changes from remote without applying them. `git pull` is `fetch + merge`. Fetch is safer — you review before merging.

**Q: How do you resolve merge conflicts?**
A: Open conflicted files, look for `<<<<<<<`, `=======`, `>>>>>>>` markers, manually edit to keep the correct code, remove markers, then `git add` and `git commit`.

### Mid Level

**Q: Explain git rebase vs git merge with use cases.**
A: Merge preserves complete history with merge commits, good for public branches. Rebase replays commits creating linear history, good for local feature branches before PR. Never rebase public shared branches.

**Q: How would you squash the last 5 commits into one?**
A: `git rebase -i HEAD~5`, change `pick` to `squash` for commits 2-5, write combined message.

**Q: What's the difference between git reset --soft, --mixed, and --hard?**
A: Soft: undo commit, keep staged. Mixed (default): undo commit, keep in working dir. Hard: undo commit, delete changes. All three move the branch pointer — only use on unpushed commits.

### Senior Level

**Q: How would you find which commit introduced a bug in a codebase with 10,000 commits?**
A: Use `git bisect` with automated testing: `git bisect start`, mark bad/good bounds, then `git bisect run npm test` to automatically binary search through commits.

**Q: A developer accidentally pushed AWS credentials to a public GitHub repo. Walk me through your incident response.**
A: (1) Rotate the credentials immediately via AWS IAM. (2) Make repo private. (3) Use git-filter-repo to remove from history. (4) Force push cleaned history. (5) Have all team members re-clone. (6) Set up pre-commit hooks with secret scanning. (7) Post-mortem.

**Q: Compare GitFlow and Trunk Based Development for a team releasing multiple times per day.**
A: TBD is better for high-frequency releases. GitFlow introduces overhead (multiple long-lived branches, complex merge paths) that slows down. TBD with feature flags and CI/CD enables multiple daily deploys.

---

<a name="cheat-sheet"></a>
## 📋 COMPLETE CHEAT SHEET

### Setup
```bash
git config --global user.name "Name"
git config --global user.email "email"
git config --global init.defaultBranch main
```

### Basics
```bash
git init | git clone url
git status | git status -s
git add . | git add -p
git commit -m "msg" | git commit --amend
git log --oneline --graph | git log -10
git diff | git diff --staged
```

### Branching
```bash
git branch -a | git branch -v
git switch -c feature/name
git switch main
git branch -d branch | git branch -D branch
```

### Merging & Rebasing
```bash
git merge branch --no-ff
git merge --abort
git rebase main
git rebase -i HEAD~3
git rebase --continue | --abort
```

### Remote
```bash
git remote -v
git remote add origin url
git push -u origin main
git push --force-with-lease
git fetch origin
git pull --rebase
```

### Undo
```bash
git restore file                    # Discard changes
git restore --staged file           # Unstage
git reset --soft HEAD~1             # Undo commit, keep staged
git reset --hard HEAD~1             # Undo commit, delete changes
git revert abc1234                  # Safe undo (new commit)
git reflog                          # Recover anything
```

### Stash
```bash
git stash | git stash push -m "msg"
git stash list | git stash show -p
git stash pop | git stash apply
git stash drop | git stash clear
```

### Tags
```bash
git tag -a v1.0.0 -m "Release"
git push origin --tags
git tag -d v1.0.0
```

### Debugging
```bash
git bisect start/bad/good/run
git cherry-pick abc1234
git blame file.txt
git log -S "search string"
git reflog
git fsck
```

### GitHub Actions
```yaml
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - run: your-command
```

---

<a name="roadmap"></a>
## 🗺️ YOUR LEARNING ROADMAP

```
WEEK 1-2: BEGINNER FOUNDATIONS
─────────────────────────────
□ Install Git, configure identity
□ init, clone, add, commit, status
□ log, diff, .gitignore
□ restore, reset basics
□ Daily habit: commit real code with good messages

WEEK 3-4: INTERMEDIATE SKILLS  
──────────────────────────────
□ Branching + merging workflow
□ Resolve 3 intentional merge conflicts
□ Master git stash
□ Understand rebase (linear history)
□ revert vs reset decision making

WEEK 5-6: GITHUB WORKFLOW
─────────────────────────
□ Set up SSH keys
□ Create and review 5 Pull Requests
□ Set up branch protection rules
□ Create first GitHub Actions workflow
□ Practice fork + upstream sync

WEEK 7-8: DEVOPS INTEGRATION
─────────────────────────────
□ Implement GitFlow OR Trunk-Based in project
□ Full CI/CD pipeline (test + build + deploy)
□ Set up pre-commit hooks
□ Practice git bisect on a real bug
□ Cherry-pick across branches
□ Implement secret scanning

WEEK 9-10: EXPERT TOPICS
─────────────────────────
□ Explore git internals (cat-file, hash-object)
□ Practice disaster recovery scenarios
□ Set up git filter-repo
□ GPG sign your commits
□ Performance optimization on large repo
□ Master git reflog recovery

CERTIFICATIONS & NEXT STEPS:
─────────────────────────────
□ GitHub Foundations Certification
□ GitHub Actions Certification
□ Build a portfolio of CI/CD projects
□ Contribute to open source (real PR workflow)
□ Linux + Docker + Kubernetes (complete DevOps stack)
```

---

## 📚 RECOMMENDED RESOURCES

```
BOOKS:
□ "Pro Git" by Scott Chacon (free at git-scm.com/book)
□ "Git for Teams" by Emma Jane Hogbin Westby

INTERACTIVE LEARNING:
□ learngitbranching.js.org (visualize branching!)
□ ohmygit.org (game-based learning)
□ katacoda.com (Git scenarios)

DOCUMENTATION:
□ git-scm.com/docs (official reference)
□ docs.github.com (GitHub-specific)
□ ohshitgit.com (plain English problem solving)

PRACTICE:
□ github.com/firstcontributions/first-contributions
□ Make 1 commit per day for 30 days
□ Set up a personal project with full CI/CD
```

---

*You now have a complete Git & GitHub mastery guide. Every concept used in real DevOps production environments. The key is practice — commit code every day, open real PRs, and build real pipelines. You've got this. 🔀*
