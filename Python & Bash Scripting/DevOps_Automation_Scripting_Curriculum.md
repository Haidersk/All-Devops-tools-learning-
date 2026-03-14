# DevOps Automation Scripting — Complete Engineering Curriculum
## Python + Bash for Production DevOps Automation

> **Covers:** Python CLI Scripting · OS Automation · Shell Commands · Deployment Scripts · API Automation · JSON/YAML · Bash Fundamentals · System Monitoring · Log Analysis · Kubernetes Automation · AWS Automation · CI/CD Scripting
> **Track:** DevOps Automation Career Path | Beginner → Senior Engineer
> **Based on:** Python Docs · GNU Bash Manual · Linux Command Reference

---

## How to Use This Document

- Every script is production-ready — not toy examples
- Run every script yourself and break it intentionally — that is how you learn edge cases
- Each lab builds on the previous — don't skip
- Real DevOps work is 80% automation scripting — this document is your reference

---

## Table of Contents

### PHASE 1 — Python for DevOps
- [Lesson 1 — Python CLI Scripting and DevOps Foundations](#lesson-1--python-cli-scripting-and-devops-foundations)
- [Lesson 2 — Working with the OS Module and File System Automation](#lesson-2--working-with-the-os-module-and-file-system-automation)
- [Lesson 3 — Running Shell Commands from Python](#lesson-3--running-shell-commands-from-python)
- [Lesson 4 — JSON and YAML Processing](#lesson-4--json-and-yaml-processing)
- [Lesson 5 — Logging in Automation Scripts](#lesson-5--logging-in-automation-scripts)
- [Lesson 6 — Working with APIs](#lesson-6--working-with-apis)
- [Lesson 7 — Automating Deployments with Python](#lesson-7--automating-deployments-with-python)
- [Lesson 8 — Infrastructure Automation — Lab Project](#lesson-8--infrastructure-automation--lab-project)

### PHASE 2 — Bash Scripting Fundamentals
- [Lesson 9 — Bash Shell Basics and Script Execution](#lesson-9--bash-shell-basics-and-script-execution)
- [Lesson 10 — Variables and Environment Variables](#lesson-10--variables-and-environment-variables)
- [Lesson 11 — Conditionals and Exit Codes](#lesson-11--conditionals-and-exit-codes)
- [Lesson 12 — Loops and Arrays](#lesson-12--loops-and-arrays)
- [Lesson 13 — Functions and Command Substitution](#lesson-13--functions-and-command-substitution)

### PHASE 3 — DevOps Bash Automation
- [Lesson 14 — System Monitoring Scripts](#lesson-14--system-monitoring-scripts)
- [Lesson 15 — Log Analysis Scripts](#lesson-15--log-analysis-scripts)
- [Lesson 16 — Server Health Check Tool](#lesson-16--server-health-check-tool)
- [Lesson 17 — Deployment Automation Scripts](#lesson-17--deployment-automation-scripts)
- [Lesson 18 — Backup Automation and Cron Jobs](#lesson-18--backup-automation-and-cron-jobs)

### PHASE 4 — Advanced DevOps Automation
- [Lesson 19 — Python AWS Automation with Boto3](#lesson-19--python-aws-automation-with-boto3)
- [Lesson 20 — Kubernetes Automation Scripts](#lesson-20--kubernetes-automation-scripts)
- [Lesson 21 — CI/CD Automation Scripting](#lesson-21--cicd-automation-scripting)
- [Lesson 22 — Configuration Automation](#lesson-22--configuration-automation)
- [Lesson 23 — Complete DevOps Automation Toolkit](#lesson-23--complete-devops-automation-toolkit)

---

---

# PHASE 1 — PYTHON FOR DEVOPS

---

## Lesson 1 — Python CLI Scripting and DevOps Foundations

> **Goal:** Build command-line tools that DevOps engineers actually use every day

---

### 1.1 Why Python for DevOps Automation?

Before writing a single line, understand the mental model. DevOps automation scripts solve one of three problems:

```
1. REPETITION   → "I do this manually every day — automate it"
2. RELIABILITY  → "Humans make mistakes — let a script be consistent"
3. SCALE        → "I can't do this 1,000 times manually — script it"
```

Python wins for DevOps automation because:
- Reads like pseudocode — ops engineers without dev backgrounds can maintain it
- Rich standard library — `subprocess`, `os`, `pathlib`, `json`, `logging` all built in
- Excellent third-party ecosystem — `boto3` (AWS), `kubernetes`, `requests`, `paramiko`
- Cross-platform — same script runs on Linux CI agents, macOS laptops, Windows

---

### 1.2 The argparse Module — Professional CLI Tools

Every production DevOps script needs proper argument parsing. No script should require you to edit the file to change its behaviour.

```python
#!/usr/bin/env python3
"""
deploy.py — Application deployment automation tool.
Usage:
    python deploy.py --app myapi --env production --version v1.2.3
    python deploy.py --app myapi --env staging --dry-run
"""
import argparse
import sys
import logging
from typing import Optional

def parse_args() -> argparse.Namespace:
    """Parse and validate command-line arguments."""
    parser = argparse.ArgumentParser(
        description="Application deployment automation",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
Examples:
  %(prog)s --app myapi --env staging --version v1.2.3
  %(prog)s --app myapi --env production --version v1.2.3 --dry-run
  %(prog)s --app myapi --env production --version v1.2.3 --timeout 600
        """,
    )

    # Required arguments
    parser.add_argument(
        "--app",
        required=True,
        help="Application name to deploy (e.g., myapi, worker, frontend)",
    )
    parser.add_argument(
        "--env",
        required=True,
        choices=["dev", "staging", "production"],
        help="Target deployment environment",
    )

    # Optional arguments with defaults
    parser.add_argument(
        "--version",
        default="latest",
        help="Docker image tag to deploy (default: latest)",
    )
    parser.add_argument(
        "--timeout",
        type=int,
        default=300,
        metavar="SECONDS",
        help="Deployment timeout in seconds (default: 300)",
    )
    parser.add_argument(
        "--replicas",
        type=int,
        default=None,
        help="Override replica count for this deployment",
    )

    # Flags (boolean switches)
    parser.add_argument(
        "--dry-run",
        action="store_true",
        help="Preview deployment plan without executing",
    )
    parser.add_argument(
        "--verbose", "-v",
        action="store_true",
        help="Enable verbose output",
    )
    parser.add_argument(
        "--no-health-check",
        action="store_true",
        help="Skip post-deployment health check",
    )

    args = parser.parse_args()

    # Custom validation — argparse can't express all constraints
    if args.env == "production" and args.version == "latest":
        parser.error(
            "Using 'latest' tag in production is dangerous. "
            "Please specify an explicit version (e.g., --version v1.2.3)"
        )

    if args.replicas is not None and args.replicas < 1:
        parser.error("--replicas must be at least 1")

    return args


def deploy(args: argparse.Namespace) -> int:
    """Execute the deployment. Returns exit code (0=success, 1=failure)."""
    log_level = logging.DEBUG if args.verbose else logging.INFO
    logging.basicConfig(
        format="%(asctime)s [%(levelname)s] %(message)s",
        level=log_level,
        datefmt="%Y-%m-%d %H:%M:%S",
    )
    logger = logging.getLogger(__name__)

    logger.info(f"Deployment plan:")
    logger.info(f"  Application : {args.app}")
    logger.info(f"  Environment : {args.env}")
    logger.info(f"  Version     : {args.version}")
    logger.info(f"  Timeout     : {args.timeout}s")
    if args.replicas:
        logger.info(f"  Replicas    : {args.replicas}")

    if args.dry_run:
        logger.info("DRY RUN — no changes will be made")
        return 0

    # Deployment logic goes here
    logger.info(f"Deploying {args.app}:{args.version} to {args.env}...")
    # ... actual deployment code ...
    logger.info("Deployment complete!")
    return 0


def main() -> None:
    args = parse_args()
    exit_code = deploy(args)
    sys.exit(exit_code)


if __name__ == "__main__":
    main()
```

```bash
# Test it
python deploy.py --help
python deploy.py --app myapi --env staging --version v1.2.3 --dry-run
python deploy.py --app myapi --env production --version latest  # Error!
```

---

### 1.3 Exit Codes — The Language of Scripts

DevOps scripts must communicate success and failure via exit codes. CI/CD pipelines, monitoring systems, and shell scripts all depend on this.

```python
import sys

# The contract:
# exit code 0  = success
# exit code 1  = general failure
# exit code 2  = misuse of shell command / bad arguments
# exit code 3+ = application-specific (document these!)

def main() -> None:
    try:
        result = do_work()
        if result.success:
            print("Operation completed successfully")
            sys.exit(0)          # ← Always explicit in scripts
        else:
            print(f"ERROR: {result.error}", file=sys.stderr)
            sys.exit(1)
    except KeyboardInterrupt:
        print("\nInterrupted by user", file=sys.stderr)
        sys.exit(130)            # Standard for Ctrl+C
    except Exception as e:
        print(f"FATAL: Unexpected error: {e}", file=sys.stderr)
        sys.exit(1)

# In shell scripts, you check: if python deploy.py; then echo "OK"; fi
# In CI/CD, a non-zero exit code fails the pipeline step
```

---

### 1.4 Script Template — Production Standard

Every DevOps script you write should follow this pattern:

```python
#!/usr/bin/env python3
"""
Script name and one-line description.

Detailed description of what this script does,
when to use it, and any important notes.

Usage:
    python script_name.py [options]

Examples:
    python script_name.py --config config.yml --env production
"""
import argparse
import logging
import sys
from pathlib import Path

# ─── CONSTANTS ────────────────────────────────────────────────────
SCRIPT_VERSION = "1.0.0"
DEFAULT_CONFIG = Path(__file__).parent / "config.yml"

# ─── LOGGING SETUP ────────────────────────────────────────────────
logger = logging.getLogger(__name__)

def setup_logging(verbose: bool = False) -> None:
    """Configure logging for the script."""
    level = logging.DEBUG if verbose else logging.INFO
    logging.basicConfig(
        level=level,
        format="%(asctime)s [%(levelname)-8s] %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S",
        handlers=[
            logging.StreamHandler(sys.stdout),
        ],
    )

# ─── ARGUMENT PARSING ─────────────────────────────────────────────
def parse_args() -> argparse.Namespace:
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument("--version", action="version", version=SCRIPT_VERSION)
    parser.add_argument("-v", "--verbose", action="store_true")
    # Add your arguments here
    return parser.parse_args()

# ─── CORE LOGIC ───────────────────────────────────────────────────
def run(args: argparse.Namespace) -> int:
    """Main logic. Returns exit code."""
    logger.info("Script started")
    # Your logic here
    return 0

# ─── ENTRY POINT ──────────────────────────────────────────────────
def main() -> None:
    args = parse_args()
    setup_logging(args.verbose)
    sys.exit(run(args))

if __name__ == "__main__":
    main()
```

---

### 1.5 Hands-On Lab 1 — Build a Deployment Status Checker

**Task:** Write a CLI tool that checks if a list of services are "deployed" (simulate with a dictionary) and reports status.

```python
#!/usr/bin/env python3
"""
service_status.py — Check deployment status of services.

Usage:
    python service_status.py --env production
    python service_status.py --env staging --service api
    python service_status.py --env production --format json
"""
import argparse
import json
import sys
from datetime import datetime

# Simulated service registry (in real life: Kubernetes API, AWS ECS, etc.)
SERVICE_REGISTRY = {
    "production": {
        "api":      {"version": "v2.1.0", "replicas": 5, "healthy": 5,  "status": "running"},
        "worker":   {"version": "v2.1.0", "replicas": 3, "healthy": 3,  "status": "running"},
        "frontend": {"version": "v2.0.5", "replicas": 2, "healthy": 1,  "status": "degraded"},
        "scheduler":{"version": "v1.9.0", "replicas": 1, "healthy": 0,  "status": "failed"},
    },
    "staging": {
        "api":      {"version": "v2.2.0-rc1", "replicas": 2, "healthy": 2, "status": "running"},
        "worker":   {"version": "v2.2.0-rc1", "replicas": 1, "healthy": 1, "status": "running"},
        "frontend": {"version": "v2.1.0-rc2", "replicas": 1, "healthy": 1, "status": "running"},
        "scheduler":{"version": "v2.0.0-rc1", "replicas": 1, "healthy": 1, "status": "running"},
    },
}

STATUS_ICONS = {
    "running":  "✅",
    "degraded": "⚠️ ",
    "failed":   "❌",
    "stopped":  "⏹ ",
}

def parse_args() -> argparse.Namespace:
    parser = argparse.ArgumentParser(description="Check service deployment status")
    parser.add_argument(
        "--env",
        required=True,
        choices=list(SERVICE_REGISTRY.keys()),
        help="Environment to check",
    )
    parser.add_argument(
        "--service",
        help="Check specific service only (default: all services)",
    )
    parser.add_argument(
        "--format",
        choices=["table", "json"],
        default="table",
        help="Output format (default: table)",
    )
    parser.add_argument(
        "--fail-on-degraded",
        action="store_true",
        help="Exit with code 1 if any service is degraded or failed",
    )
    return parser.parse_args()


def get_services(env: str, service_name: str = None) -> dict:
    """Return services for the given environment."""
    services = SERVICE_REGISTRY.get(env, {})
    if service_name:
        if service_name not in services:
            print(f"ERROR: Service '{service_name}' not found in {env}", file=sys.stderr)
            sys.exit(1)
        return {service_name: services[service_name]}
    return services


def print_table(env: str, services: dict) -> None:
    """Print status as a formatted table."""
    print(f"\n{'─' * 65}")
    print(f"  Service Status — {env.upper()}   [{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}]")
    print(f"{'─' * 65}")
    print(f"  {'SERVICE':<15} {'VERSION':<18} {'REPLICAS':>10}  {'STATUS'}")
    print(f"{'─' * 65}")

    for name, info in services.items():
        icon  = STATUS_ICONS.get(info["status"], "?")
        ratio = f"{info['healthy']}/{info['replicas']}"
        print(f"  {name:<15} {info['version']:<18} {ratio:>10}  {icon} {info['status']}")

    print(f"{'─' * 65}\n")


def print_json(env: str, services: dict) -> None:
    """Print status as JSON (for piping to other tools)."""
    output = {
        "environment": env,
        "timestamp": datetime.utcnow().isoformat() + "Z",
        "services": services,
    }
    print(json.dumps(output, indent=2))


def main() -> None:
    args = parse_args()
    services = get_services(args.env, args.service)

    if args.format == "json":
        print_json(args.env, services)
    else:
        print_table(args.env, services)

    # Exit code logic
    if args.fail_on_degraded:
        bad = [n for n, s in services.items() if s["status"] in ("degraded", "failed")]
        if bad:
            print(f"ALERT: Unhealthy services: {', '.join(bad)}", file=sys.stderr)
            sys.exit(1)

    sys.exit(0)


if __name__ == "__main__":
    main()
```

```bash
python service_status.py --env production
python service_status.py --env staging --service api
python service_status.py --env production --format json
python service_status.py --env production --fail-on-degraded; echo "Exit: $?"
```

---

---

## Lesson 2 — Working with the OS Module and File System Automation

> **Goal:** Automate file and directory operations that DevOps engineers do constantly

---

### 2.1 pathlib — The Modern Way

```python
from pathlib import Path
import shutil
import os

# ─── NAVIGATION ───────────────────────────────────────────────────
home = Path.home()                    # /home/username
cwd  = Path.cwd()                     # Current working directory
script_dir = Path(__file__).parent    # Directory containing this script

# Build paths safely (works on Windows, Mac, Linux)
config_file = home / ".config" / "myapp" / "config.yml"
log_dir      = Path("/var") / "log" / "myapp"

# ─── INSPECTION ───────────────────────────────────────────────────
p = Path("/var/log/nginx/access.log")
print(p.name)       # access.log
print(p.stem)       # access
print(p.suffix)     # .log
print(p.parent)     # /var/log/nginx
print(p.parts)      # ('/', 'var', 'log', 'nginx', 'access.log')
print(p.exists())   # True/False
print(p.is_file())  # True
print(p.is_dir())   # False

# File metadata
stat = p.stat()
print(f"Size: {stat.st_size:,} bytes")
print(f"Modified: {stat.st_mtime}")

import datetime
modified = datetime.datetime.fromtimestamp(stat.st_mtime)
print(f"Modified: {modified.strftime('%Y-%m-%d %H:%M:%S')}")

# ─── DIRECTORY OPERATIONS ─────────────────────────────────────────
# Create directory tree (no error if exists)
Path("/tmp/myapp/logs/archive").mkdir(parents=True, exist_ok=True)

# List directory contents
for item in Path("/etc").iterdir():
    print(f"{'DIR ' if item.is_dir() else 'FILE'} {item.name}")

# Find files by pattern
for log_file in Path("/var/log").glob("*.log"):
    print(log_file)

for py_file in Path(".").rglob("*.py"):    # Recursive
    print(py_file)

# ─── FILE OPERATIONS ─────────────────────────────────────────────
# Read and write
config = Path("config.yml").read_text(encoding="utf-8")
Path("config.yml.bak").write_text(config, encoding="utf-8")

# Copy
shutil.copy2("source.txt", "dest.txt")           # copy with metadata
shutil.copytree("src_dir/", "dest_dir/")          # copy directory tree

# Move/rename
Path("old_name.txt").rename("new_name.txt")
shutil.move("file.txt", "/backup/file.txt")

# Delete
Path("temp_file.txt").unlink(missing_ok=True)    # Delete file
shutil.rmtree("temp_dir/", ignore_errors=True)   # Delete directory tree
```

---

### 2.2 Real DevOps File Automation

```python
#!/usr/bin/env python3
"""
log_archiver.py — Archive log files older than N days.

Real DevOps use case: Nightly log archival to keep disk usage under control.
"""
import argparse
import gzip
import logging
import shutil
import sys
from datetime import datetime, timedelta
from pathlib import Path
from typing import List, Tuple

logger = logging.getLogger(__name__)

def find_old_logs(log_dir: Path, days: int, pattern: str) -> List[Path]:
    """Find log files older than `days` days matching `pattern`."""
    cutoff = datetime.now() - timedelta(days=days)
    old_logs = []

    for log_file in log_dir.glob(pattern):
        if not log_file.is_file():
            continue
        modified = datetime.fromtimestamp(log_file.stat().st_mtime)
        if modified < cutoff:
            old_logs.append(log_file)

    return sorted(old_logs)


def compress_file(source: Path, dest_dir: Path) -> Tuple[Path, int, int]:
    """Gzip compress a file. Returns (compressed_path, original_size, compressed_size)."""
    dest = dest_dir / (source.name + ".gz")
    original_size = source.stat().st_size

    with source.open("rb") as f_in:
        with gzip.open(dest, "wb", compresslevel=9) as f_out:
            shutil.copyfileobj(f_in, f_out)

    compressed_size = dest.stat().st_size
    return dest, original_size, compressed_size


def archive_logs(
    log_dir: Path,
    archive_dir: Path,
    days: int,
    pattern: str,
    dry_run: bool,
) -> dict:
    """Archive log files and return statistics."""
    stats = {
        "found": 0,
        "archived": 0,
        "failed": 0,
        "original_bytes": 0,
        "compressed_bytes": 0,
    }

    archive_dir.mkdir(parents=True, exist_ok=True)
    old_logs = find_old_logs(log_dir, days, pattern)
    stats["found"] = len(old_logs)

    if not old_logs:
        logger.info(f"No log files older than {days} days found in {log_dir}")
        return stats

    logger.info(f"Found {len(old_logs)} log files to archive")

    for log_file in old_logs:
        logger.debug(f"Processing: {log_file}")

        if dry_run:
            size = log_file.stat().st_size
            logger.info(f"  [DRY RUN] Would archive: {log_file.name} ({size:,} bytes)")
            continue

        try:
            dest, orig_size, comp_size = compress_file(log_file, archive_dir)
            log_file.unlink()    # Delete original after successful compression

            ratio = (1 - comp_size / orig_size) * 100
            logger.info(
                f"  Archived: {log_file.name} → {dest.name} "
                f"({orig_size:,} → {comp_size:,} bytes, {ratio:.0f}% reduction)"
            )

            stats["archived"] += 1
            stats["original_bytes"] += orig_size
            stats["compressed_bytes"] += comp_size

        except Exception as e:
            logger.error(f"  FAILED to archive {log_file.name}: {e}")
            stats["failed"] += 1

    return stats


def main() -> None:
    parser = argparse.ArgumentParser(description="Archive old log files")
    parser.add_argument("--log-dir", type=Path, required=True, help="Directory containing logs")
    parser.add_argument("--archive-dir", type=Path, required=True, help="Archive destination")
    parser.add_argument("--days", type=int, default=7, help="Archive logs older than N days (default: 7)")
    parser.add_argument("--pattern", default="*.log", help="Log file pattern (default: *.log)")
    parser.add_argument("--dry-run", action="store_true", help="Preview without changes")
    parser.add_argument("-v", "--verbose", action="store_true")
    args = parser.parse_args()

    logging.basicConfig(
        level=logging.DEBUG if args.verbose else logging.INFO,
        format="%(asctime)s [%(levelname)s] %(message)s",
    )

    if not args.log_dir.exists():
        logger.error(f"Log directory does not exist: {args.log_dir}")
        sys.exit(1)

    stats = archive_logs(
        args.log_dir, args.archive_dir, args.days, args.pattern, args.dry_run
    )

    print(f"\n{'─' * 40}")
    print(f"  Archive Summary")
    print(f"{'─' * 40}")
    print(f"  Found    : {stats['found']} files")
    if not args.dry_run:
        saved = stats["original_bytes"] - stats["compressed_bytes"]
        print(f"  Archived : {stats['archived']} files")
        print(f"  Failed   : {stats['failed']} files")
        if stats["original_bytes"] > 0:
            print(f"  Space saved: {saved / 1024 / 1024:.1f} MB")

    sys.exit(1 if stats["failed"] > 0 else 0)


if __name__ == "__main__":
    main()
```

```bash
# Create test data
mkdir -p /tmp/test-logs /tmp/test-archive
for i in {1..5}; do
    echo "Log data $i" > /tmp/test-logs/app-$i.log
    touch -d "-${i}0 days" /tmp/test-logs/app-$i.log
done

# Run
python log_archiver.py \
    --log-dir /tmp/test-logs \
    --archive-dir /tmp/test-archive \
    --days 7 \
    --verbose
```

---

---

## Lesson 3 — Running Shell Commands from Python

> **Goal:** Execute system commands from Python scripts safely and reliably

---

### 3.1 The subprocess Module — Complete Reference

```python
import subprocess
import shlex
import sys

# ─── BASIC EXECUTION ──────────────────────────────────────────────
# subprocess.run() — the modern standard

# Simple command
result = subprocess.run(["ls", "-la"], capture_output=True, text=True)
print(result.stdout)
print(result.returncode)    # 0 = success

# Check return code automatically (raises CalledProcessError on failure)
result = subprocess.run(
    ["git", "status"],
    capture_output=True,
    text=True,
    check=True,              # ← raises on non-zero exit
)

# With timeout
result = subprocess.run(
    ["ping", "-c", "3", "google.com"],
    capture_output=True,
    text=True,
    timeout=10,              # Raise TimeoutExpired after 10s
)

# ─── SAFE COMMAND CONSTRUCTION ────────────────────────────────────
# NEVER use shell=True with user input — shell injection risk!
user_filename = "../../etc/passwd"   # Attacker input

# DANGEROUS:
# subprocess.run(f"cat {user_filename}", shell=True)  ← NEVER DO THIS

# SAFE — shell=False (default) with list
result = subprocess.run(
    ["cat", user_filename],   # Shell never sees the filename raw
    capture_output=True,
    text=True,
)

# If you must parse a command string, use shlex.split
command_str = "git log --oneline -10"
result = subprocess.run(
    shlex.split(command_str),   # Proper shell-like splitting
    capture_output=True,
    text=True,
)

# ─── PRACTICAL WRAPPER ────────────────────────────────────────────
def run_command(
    cmd: list,
    cwd: str = None,
    env: dict = None,
    timeout: int = 60,
    check: bool = True,
) -> subprocess.CompletedProcess:
    """
    Run a shell command with standard error handling.

    Args:
        cmd: Command as list of strings (e.g., ["git", "pull"])
        cwd: Working directory
        env: Environment variables (None = inherit current env)
        timeout: Timeout in seconds
        check: Raise exception on non-zero exit code

    Returns:
        CompletedProcess with .stdout, .stderr, .returncode

    Raises:
        subprocess.CalledProcessError: If check=True and command fails
        subprocess.TimeoutExpired: If command exceeds timeout
    """
    import logging
    logger = logging.getLogger(__name__)

    logger.debug(f"Running: {' '.join(cmd)}")
    if cwd:
        logger.debug(f"  Working dir: {cwd}")

    try:
        result = subprocess.run(
            cmd,
            capture_output=True,
            text=True,
            cwd=cwd,
            env=env,
            timeout=timeout,
            check=check,
        )
        if result.stdout:
            logger.debug(f"  stdout: {result.stdout.strip()}")
        return result

    except subprocess.CalledProcessError as e:
        logger.error(f"Command failed (exit {e.returncode}): {' '.join(cmd)}")
        if e.stdout:
            logger.error(f"  stdout: {e.stdout.strip()}")
        if e.stderr:
            logger.error(f"  stderr: {e.stderr.strip()}")
        raise

    except subprocess.TimeoutExpired:
        logger.error(f"Command timed out after {timeout}s: {' '.join(cmd)}")
        raise
```

---

### 3.2 Streaming Output — For Long-Running Commands

```python
import subprocess
import sys

def run_with_streaming(cmd: list, prefix: str = "") -> int:
    """
    Run a command and stream output to stdout in real time.
    Perfect for docker build, terraform apply, test runners.
    """
    process = subprocess.Popen(
        cmd,
        stdout=subprocess.PIPE,
        stderr=subprocess.STDOUT,   # Merge stderr into stdout
        text=True,
        bufsize=1,                  # Line-buffered
    )

    for line in process.stdout:
        line = line.rstrip()
        if prefix:
            print(f"{prefix} | {line}")
        else:
            print(line)
        sys.stdout.flush()

    process.wait()
    return process.returncode


# Example: stream docker build output
exit_code = run_with_streaming(
    ["docker", "build", "-t", "myapp:latest", "."],
    prefix="docker-build"
)

if exit_code != 0:
    print(f"ERROR: Docker build failed (exit {exit_code})", file=sys.stderr)
    sys.exit(1)
```

---

### 3.3 Real DevOps Script — Git Operations Automation

```python
#!/usr/bin/env python3
"""
git_ops.py — Automate common Git operations for CI/CD.

Used by: CI pipelines to tag releases, update changelogs, push release commits.
"""
import argparse
import logging
import subprocess
import sys
from typing import Optional

logger = logging.getLogger(__name__)


def run_git(args: list, cwd: str = None) -> subprocess.CompletedProcess:
    """Run a git command, raise on failure."""
    return subprocess.run(
        ["git"] + args,
        capture_output=True,
        text=True,
        cwd=cwd,
        check=True,
    )


def get_current_branch() -> str:
    result = run_git(["rev-parse", "--abbrev-ref", "HEAD"])
    return result.stdout.strip()


def get_latest_tag() -> Optional[str]:
    try:
        result = run_git(["describe", "--tags", "--abbrev=0"])
        return result.stdout.strip()
    except subprocess.CalledProcessError:
        return None


def get_commit_messages_since(tag: Optional[str]) -> list:
    """Get all commit messages since last tag (or all commits if no tag)."""
    cmd = ["log", "--oneline"]
    if tag:
        cmd.append(f"{tag}..HEAD")
    result = run_git(cmd)
    return result.stdout.strip().splitlines()


def create_release_tag(version: str, message: str, push: bool) -> None:
    """Create an annotated release tag."""
    logger.info(f"Creating tag: {version}")
    run_git(["tag", "-a", version, "-m", message])

    if push:
        logger.info(f"Pushing tag {version} to origin...")
        run_git(["push", "origin", version])
        logger.info("Tag pushed successfully")


def main() -> None:
    parser = argparse.ArgumentParser(description="Git release automation")
    subparsers = parser.add_subparsers(dest="command", required=True)

    # Status subcommand
    status_p = subparsers.add_parser("status", help="Show release status")

    # Tag subcommand
    tag_p = subparsers.add_parser("tag", help="Create a release tag")
    tag_p.add_argument("--version", required=True, help="Version tag (e.g., v1.2.3)")
    tag_p.add_argument("--message", default=None, help="Tag message")
    tag_p.add_argument("--push", action="store_true", help="Push tag to origin")
    tag_p.add_argument("--dry-run", action="store_true")

    parser.add_argument("-v", "--verbose", action="store_true")
    args = parser.parse_args()

    logging.basicConfig(
        level=logging.DEBUG if args.verbose else logging.INFO,
        format="%(asctime)s [%(levelname)s] %(message)s",
    )

    branch = get_current_branch()
    latest_tag = get_latest_tag()
    commits = get_commit_messages_since(latest_tag)

    if args.command == "status":
        print(f"\nBranch     : {branch}")
        print(f"Latest tag : {latest_tag or 'none'}")
        print(f"New commits: {len(commits)}")
        if commits:
            print("\nCommits since last tag:")
            for c in commits:
                print(f"  {c}")

    elif args.command == "tag":
        if not commits:
            logger.warning("No commits since last tag — are you sure?")

        message = args.message or f"Release {args.version}"
        logger.info(f"Creating release {args.version}")
        logger.info(f"Branch: {branch} | Previous tag: {latest_tag or 'none'}")
        logger.info(f"Commits in this release: {len(commits)}")

        if args.dry_run:
            print(f"[DRY RUN] Would create tag '{args.version}' with message: '{message}'")
            sys.exit(0)

        create_release_tag(args.version, message, args.push)
        logger.info(f"Release {args.version} created successfully!")

    sys.exit(0)


if __name__ == "__main__":
    main()
```

---

---

## Lesson 4 — JSON and YAML Processing

> **Goal:** Parse, transform, and generate configuration files used throughout DevOps

---

### 4.1 JSON Processing

```python
import json
from pathlib import Path
from typing import Any

# ─── READ AND WRITE JSON ──────────────────────────────────────────
# From string
data = json.loads('{"name": "myapp", "version": "1.0.0"}')

# From file
with open("config.json") as f:
    config = json.load(f)

# To string
json_str = json.dumps(data, indent=2, sort_keys=True)

# To file
with open("output.json", "w") as f:
    json.dump(data, f, indent=2, sort_keys=True)

# ─── PATCHING JSON CONFIG ─────────────────────────────────────────
def update_json_config(filepath: Path, updates: dict) -> dict:
    """Update specific keys in a JSON file without touching others."""
    config = json.loads(filepath.read_text())

    def deep_update(base: dict, updates: dict) -> dict:
        for key, value in updates.items():
            if isinstance(value, dict) and isinstance(base.get(key), dict):
                deep_update(base[key], value)
            else:
                base[key] = value
        return base

    updated = deep_update(config, updates)
    filepath.write_text(json.dumps(updated, indent=2))
    return updated


# ─── QUERYING JSON — jq-style in Python ───────────────────────────
import json

kubernetes_pods = json.loads("""
{
  "items": [
    {"metadata": {"name": "api-abc123", "namespace": "production"},
     "status": {"phase": "Running", "podIP": "10.0.1.5"}},
    {"metadata": {"name": "api-def456", "namespace": "production"},
     "status": {"phase": "Running", "podIP": "10.0.1.6"}},
    {"metadata": {"name": "worker-xyz789", "namespace": "production"},
     "status": {"phase": "Pending", "podIP": null}}
  ]
}
""")

# Extract all pod names
names = [pod["metadata"]["name"] for pod in kubernetes_pods["items"]]

# Find pending pods
pending = [
    pod["metadata"]["name"]
    for pod in kubernetes_pods["items"]
    if pod["status"]["phase"] == "Pending"
]

# Get IPs of running pods
running_ips = [
    pod["status"]["podIP"]
    for pod in kubernetes_pods["items"]
    if pod["status"]["phase"] == "Running" and pod["status"]["podIP"]
]
```

---

### 4.2 YAML Processing — The DevOps Config Language

```bash
pip install pyyaml
```

```python
import yaml
from pathlib import Path

# ─── READ YAML ────────────────────────────────────────────────────
with open("docker-compose.yml") as f:
    compose = yaml.safe_load(f)    # safe_load — never use yaml.load() (unsafe!)

# Multi-document YAML (Kubernetes manifests)
with open("k8s-manifests.yml") as f:
    docs = list(yaml.safe_load_all(f))

# ─── WRITE YAML ───────────────────────────────────────────────────
data = {
    "apiVersion": "apps/v1",
    "kind": "Deployment",
    "metadata": {"name": "myapp", "namespace": "production"},
    "spec": {"replicas": 3},
}

with open("deployment.yml", "w") as f:
    yaml.dump(data, f, default_flow_style=False, allow_unicode=True)

# ─── PATCH KUBERNETES MANIFESTS ───────────────────────────────────
def update_image_tag(manifest_path: Path, image_name: str, new_tag: str) -> bool:
    """
    Update Docker image tag in a Kubernetes deployment manifest.

    Returns True if image was found and updated.
    """
    with open(manifest_path) as f:
        manifest = yaml.safe_load(f)

    updated = False
    containers = (
        manifest.get("spec", {})
        .get("template", {})
        .get("spec", {})
        .get("containers", [])
    )

    for container in containers:
        current_image = container.get("image", "")
        image_repo = current_image.split(":")[0]

        if image_repo == image_name or image_repo.endswith(f"/{image_name}"):
            old_image = container["image"]
            container["image"] = f"{image_repo}:{new_tag}"
            print(f"Updated: {old_image} → {container['image']}")
            updated = True

    if updated:
        with open(manifest_path, "w") as f:
            yaml.dump(manifest, f, default_flow_style=False)

    return updated


# Usage in CI/CD pipeline:
# update_image_tag(Path("k8s/deployment.yml"), "mycompany/api", "v2.1.0-abc123")
```

---

### 4.3 Complete Config Management Tool

```python
#!/usr/bin/env python3
"""
config_manager.py — Merge environment-specific configs with base config.

DevOps use case: Maintain base config and environment overlays.

Directory structure:
    configs/
    ├── base.yml           ← Default values
    ├── dev.yml            ← Dev overrides
    ├── staging.yml        ← Staging overrides
    └── production.yml     ← Production overrides
"""
import argparse
import json
import sys
from pathlib import Path
from typing import Any, Dict
import yaml


def deep_merge(base: dict, override: dict) -> dict:
    """Recursively merge override into base dict."""
    result = base.copy()
    for key, value in override.items():
        if key in result and isinstance(result[key], dict) and isinstance(value, dict):
            result[key] = deep_merge(result[key], value)
        else:
            result[key] = value
    return result


def load_config(environment: str, config_dir: Path) -> dict:
    """Load merged config for the given environment."""
    base_file = config_dir / "base.yml"
    env_file  = config_dir / f"{environment}.yml"

    if not base_file.exists():
        raise FileNotFoundError(f"Base config not found: {base_file}")

    with open(base_file) as f:
        config = yaml.safe_load(f) or {}

    if env_file.exists():
        with open(env_file) as f:
            env_overrides = yaml.safe_load(f) or {}
        config = deep_merge(config, env_overrides)
    else:
        print(f"Warning: No environment config found: {env_file}", file=sys.stderr)

    return config


def main() -> None:
    parser = argparse.ArgumentParser(description="Merge environment configs")
    parser.add_argument("--env", required=True, choices=["dev", "staging", "production"])
    parser.add_argument("--config-dir", type=Path, default=Path("configs"))
    parser.add_argument("--format", choices=["yaml", "json", "env"], default="yaml")
    parser.add_argument("--output", type=Path, help="Write to file (default: stdout)")
    args = parser.parse_args()

    config = load_config(args.env, args.config_dir)

    if args.format == "yaml":
        output = yaml.dump(config, default_flow_style=False)
    elif args.format == "json":
        output = json.dumps(config, indent=2)
    elif args.format == "env":
        # Output as KEY=VALUE pairs for Docker --env-file
        lines = []
        def flatten(obj, prefix=""):
            for k, v in obj.items():
                key = f"{prefix}_{k}".upper() if prefix else k.upper()
                if isinstance(v, dict):
                    flatten(v, key)
                else:
                    lines.append(f"{key}={v}")
        flatten(config)
        output = "\n".join(lines)

    if args.output:
        args.output.write_text(output)
        print(f"Config written to: {args.output}")
    else:
        print(output)


if __name__ == "__main__":
    main()
```

---

---

## Lesson 5 — Logging in Automation Scripts

> **Goal:** Build scripts that tell you exactly what happened and when

---

### 5.1 Production Logging Setup

```python
import logging
import logging.handlers
import sys
from pathlib import Path
from datetime import datetime


def configure_logging(
    name: str = "automation",
    level: str = "INFO",
    log_file: Path = None,
    max_bytes: int = 10 * 1024 * 1024,  # 10MB
    backup_count: int = 5,
    json_format: bool = False,
) -> logging.Logger:
    """
    Configure logging for production automation scripts.

    Features:
    - Coloured console output (INFO=white, WARNING=yellow, ERROR=red)
    - Rotating file handler (optional)
    - JSON format for log aggregation systems (optional)
    """
    logger = logging.getLogger(name)
    logger.setLevel(getattr(logging, level.upper()))
    logger.handlers.clear()

    if json_format:
        # JSON format — for Elasticsearch, Splunk, CloudWatch
        class JSONFormatter(logging.Formatter):
            def format(self, record):
                import json
                log_data = {
                    "timestamp": datetime.utcnow().isoformat() + "Z",
                    "level": record.levelname,
                    "logger": record.name,
                    "message": record.getMessage(),
                    "module": record.module,
                    "function": record.funcName,
                    "line": record.lineno,
                }
                if record.exc_info:
                    log_data["exception"] = self.formatException(record.exc_info)
                return json.dumps(log_data)
        formatter = JSONFormatter()
    else:
        # Human-readable format
        formatter = logging.Formatter(
            fmt="%(asctime)s [%(levelname)-8s] %(name)s: %(message)s",
            datefmt="%Y-%m-%d %H:%M:%S",
        )

    # Console handler
    console = logging.StreamHandler(sys.stdout)
    console.setFormatter(formatter)
    logger.addHandler(console)

    # File handler (rotating)
    if log_file:
        log_file.parent.mkdir(parents=True, exist_ok=True)
        file_handler = logging.handlers.RotatingFileHandler(
            log_file,
            maxBytes=max_bytes,
            backupCount=backup_count,
            encoding="utf-8",
        )
        file_handler.setFormatter(formatter)
        logger.addHandler(file_handler)

    return logger


# ─── USAGE IN SCRIPTS ─────────────────────────────────────────────
logger = configure_logging(
    name="deploy",
    level="DEBUG",
    log_file=Path("/var/log/automation/deploy.log"),
)

def deploy_service(name: str, version: str) -> bool:
    """Deploy a service."""
    logger.info(f"Starting deployment: {name} @ {version}")

    try:
        logger.debug(f"Pulling image {name}:{version}")
        # ... pull image ...

        logger.debug("Updating Kubernetes deployment")
        # ... update deployment ...

        logger.info(f"Deployment successful: {name} @ {version}")
        return True

    except Exception as e:
        logger.error(f"Deployment failed: {name} @ {version}: {e}", exc_info=True)
        return False
```

---

### 5.2 Structured Context Logging

```python
import logging
import contextvars

# Context variable — automatically included in all log messages
request_id: contextvars.ContextVar[str] = contextvars.ContextVar("request_id", default="-")
deployment_id: contextvars.ContextVar[str] = contextvars.ContextVar("deployment_id", default="-")


class ContextFilter(logging.Filter):
    """Adds context variables to every log record."""
    def filter(self, record):
        record.request_id = request_id.get()
        record.deployment_id = deployment_id.get()
        return True


def setup_context_logging():
    logger = logging.getLogger("automation")
    formatter = logging.Formatter(
        "%(asctime)s [%(levelname)s] deploy=%(deployment_id)s req=%(request_id)s | %(message)s"
    )
    handler = logging.StreamHandler()
    handler.setFormatter(formatter)
    handler.addFilter(ContextFilter())
    logger.addHandler(handler)
    logger.setLevel(logging.DEBUG)
    return logger


logger = setup_context_logging()


def process_deployment(app_name: str, version: str):
    import uuid
    # Set context for all log messages in this deployment
    deployment_id.set(f"{app_name}-{version[:7]}")
    request_id.set(str(uuid.uuid4())[:8])

    logger.info("Deployment started")
    logger.info("Pulling image...")
    logger.info("Updating replicas...")
    logger.info("Running health check...")
    logger.info("Deployment complete")


process_deployment("api", "v1.2.3-abc1234")
# Output:
# 2024-01-15 14:30:01 [INFO] deploy=api-v1.2.3-a req=f3a9b2c1 | Deployment started
# 2024-01-15 14:30:01 [INFO] deploy=api-v1.2.3-a req=f3a9b2c1 | Pulling image...
```

---

---

## Lesson 6 — Working with APIs

> **Goal:** Interact with REST APIs, webhooks, and cloud APIs from automation scripts

---

### 6.1 requests — The HTTP Library for DevOps

```bash
pip install requests
```

```python
import requests
import time
import logging
from typing import Any, Optional

logger = logging.getLogger(__name__)


class APIClient:
    """
    A resilient REST API client with:
    - Automatic retry with exponential backoff
    - Request/response logging
    - Consistent error handling
    - Session reuse (connection pooling)
    """

    def __init__(
        self,
        base_url: str,
        api_key: str = None,
        timeout: int = 30,
        max_retries: int = 3,
    ):
        self.base_url = base_url.rstrip("/")
        self.timeout = timeout
        self.max_retries = max_retries

        self.session = requests.Session()
        self.session.headers.update({
            "Content-Type": "application/json",
            "Accept": "application/json",
            "User-Agent": "automation-script/1.0",
        })
        if api_key:
            self.session.headers["Authorization"] = f"Bearer {api_key}"

    def _request(
        self,
        method: str,
        endpoint: str,
        **kwargs,
    ) -> requests.Response:
        """Make an HTTP request with retry logic."""
        url = f"{self.base_url}/{endpoint.lstrip('/')}"

        for attempt in range(1, self.max_retries + 1):
            try:
                logger.debug(f"→ {method} {url}")
                response = self.session.request(
                    method,
                    url,
                    timeout=self.timeout,
                    **kwargs,
                )
                logger.debug(f"← {response.status_code} {url}")

                # Retry on 429 (rate limited) or 5xx server errors
                if response.status_code == 429 or response.status_code >= 500:
                    if attempt < self.max_retries:
                        retry_after = int(response.headers.get("Retry-After", 2 ** attempt))
                        logger.warning(
                            f"  HTTP {response.status_code} — retry {attempt}/{self.max_retries} "
                            f"in {retry_after}s"
                        )
                        time.sleep(retry_after)
                        continue

                response.raise_for_status()
                return response

            except requests.ConnectionError as e:
                if attempt < self.max_retries:
                    delay = 2 ** attempt
                    logger.warning(f"  Connection failed — retry {attempt}/{self.max_retries} in {delay}s")
                    time.sleep(delay)
                else:
                    raise

        raise RuntimeError(f"All {self.max_retries} attempts failed for {url}")

    def get(self, endpoint: str, params: dict = None) -> Any:
        r = self._request("GET", endpoint, params=params)
        return r.json()

    def post(self, endpoint: str, data: dict = None) -> Any:
        r = self._request("POST", endpoint, json=data)
        return r.json()

    def patch(self, endpoint: str, data: dict = None) -> Any:
        r = self._request("PATCH", endpoint, json=data)
        return r.json()

    def delete(self, endpoint: str) -> bool:
        r = self._request("DELETE", endpoint)
        return r.status_code in (200, 204)
```

---

### 6.2 Real DevOps API Automation — GitHub and Slack

```python
#!/usr/bin/env python3
"""
notify_deployment.py — Send deployment notifications via Slack and create GitHub deployments.

Usage:
    python notify_deployment.py \
        --app myapi \
        --env production \
        --version v1.2.3 \
        --status success
"""
import argparse
import os
import sys
import requests
import logging

logger = logging.getLogger(__name__)


def send_slack_notification(
    webhook_url: str,
    app: str,
    env: str,
    version: str,
    status: str,
    details: str = "",
) -> bool:
    """Send a deployment notification to Slack."""

    color_map = {
        "success": "#36a64f",   # Green
        "failure": "#ff0000",   # Red
        "in_progress": "#f0a30a",  # Yellow
    }

    icon_map = {
        "success": "✅",
        "failure": "❌",
        "in_progress": "🚀",
    }

    payload = {
        "attachments": [
            {
                "color": color_map.get(status, "#808080"),
                "pretext": f"{icon_map.get(status, '📦')} *Deployment {status.replace('_', ' ').title()}*",
                "fields": [
                    {"title": "Application", "value": app,     "short": True},
                    {"title": "Environment", "value": env,     "short": True},
                    {"title": "Version",     "value": version, "short": True},
                    {"title": "Status",      "value": status,  "short": True},
                ],
                "footer": "Deployment Automation",
                "ts": __import__("time").time(),
            }
        ]
    }

    if details:
        payload["attachments"][0]["text"] = details

    response = requests.post(
        webhook_url,
        json=payload,
        timeout=10,
    )

    if response.status_code != 200:
        logger.error(f"Slack notification failed: {response.status_code} — {response.text}")
        return False

    logger.info("Slack notification sent")
    return True


def main() -> None:
    parser = argparse.ArgumentParser(description="Send deployment notifications")
    parser.add_argument("--app",     required=True, help="Application name")
    parser.add_argument("--env",     required=True, help="Environment")
    parser.add_argument("--version", required=True, help="Version/tag")
    parser.add_argument("--status",  required=True,
                        choices=["in_progress", "success", "failure"])
    parser.add_argument("--details", default="", help="Optional details message")
    args = parser.parse_args()

    logging.basicConfig(level=logging.INFO,
                        format="%(asctime)s [%(levelname)s] %(message)s")

    slack_webhook = os.environ.get("SLACK_WEBHOOK_URL")
    if not slack_webhook:
        logger.error("SLACK_WEBHOOK_URL environment variable not set")
        sys.exit(1)

    success = send_slack_notification(
        slack_webhook, args.app, args.env, args.version,
        args.status, args.details
    )
    sys.exit(0 if success else 1)


if __name__ == "__main__":
    main()
```

---

---

## Lesson 7 — Automating Deployments with Python

> **Goal:** Build a complete deployment automation system

---

### 7.1 The Deployment Orchestrator

```python
#!/usr/bin/env python3
"""
deploy_orchestrator.py — Full deployment workflow automation.

Orchestrates: pre-checks → deploy → health verify → notify
"""
import argparse
import logging
import subprocess
import sys
import time
from dataclasses import dataclass, field
from pathlib import Path
from typing import List, Optional
import requests

logger = logging.getLogger(__name__)


@dataclass
class DeploymentConfig:
    app_name: str
    environment: str
    image_tag: str
    namespace: str
    replicas: int = 3
    health_url: str = ""
    timeout: int = 300
    dry_run: bool = False


@dataclass
class DeploymentResult:
    success: bool
    app_name: str
    environment: str
    image_tag: str
    duration_seconds: float
    steps: List[dict] = field(default_factory=list)
    error: Optional[str] = None

    def add_step(self, name: str, status: str, message: str = ""):
        self.steps.append({
            "name": name,
            "status": status,
            "message": message,
        })


def run_kubectl(args: list, dry_run: bool = False) -> subprocess.CompletedProcess:
    """Run kubectl command."""
    cmd = ["kubectl"] + args
    if dry_run:
        logger.info(f"[DRY RUN] kubectl {' '.join(args)}")
        import subprocess
        return subprocess.CompletedProcess(cmd, 0, stdout="", stderr="")
    return subprocess.run(cmd, capture_output=True, text=True, check=True)


def pre_deployment_checks(config: DeploymentConfig) -> bool:
    """Run pre-deployment validation checks."""
    logger.info("Running pre-deployment checks...")
    checks_passed = True

    # Check 1: Kubernetes cluster is reachable
    try:
        run_kubectl(["cluster-info"])
        logger.info("  ✅ Kubernetes cluster reachable")
    except (subprocess.CalledProcessError, FileNotFoundError) as e:
        logger.error(f"  ❌ Kubernetes cluster unreachable: {e}")
        checks_passed = False

    # Check 2: Namespace exists
    try:
        run_kubectl(["get", "namespace", config.namespace])
        logger.info(f"  ✅ Namespace '{config.namespace}' exists")
    except subprocess.CalledProcessError:
        logger.error(f"  ❌ Namespace '{config.namespace}' not found")
        checks_passed = False

    # Check 3: Deployment exists
    try:
        run_kubectl(["get", "deployment", config.app_name, "-n", config.namespace])
        logger.info(f"  ✅ Deployment '{config.app_name}' exists")
    except subprocess.CalledProcessError:
        logger.error(f"  ❌ Deployment '{config.app_name}' not found in {config.namespace}")
        checks_passed = False

    return checks_passed


def update_deployment(config: DeploymentConfig) -> bool:
    """Update the Kubernetes deployment image."""
    logger.info(f"Updating deployment image to {config.image_tag}...")

    image = f"myregistry.azurecr.io/{config.app_name}:{config.image_tag}"

    try:
        run_kubectl([
            "set", "image",
            f"deployment/{config.app_name}",
            f"{config.app_name}={image}",
            "-n", config.namespace,
        ], dry_run=config.dry_run)
        logger.info(f"  Image updated to: {image}")
        return True
    except subprocess.CalledProcessError as e:
        logger.error(f"  Failed to update image: {e.stderr}")
        return False


def wait_for_rollout(config: DeploymentConfig) -> bool:
    """Wait for deployment rollout to complete."""
    logger.info(f"Waiting for rollout (timeout: {config.timeout}s)...")

    if config.dry_run:
        logger.info("  [DRY RUN] Skipping rollout wait")
        return True

    try:
        result = run_kubectl([
            "rollout", "status",
            f"deployment/{config.app_name}",
            "-n", config.namespace,
            f"--timeout={config.timeout}s",
        ])
        logger.info("  Rollout completed successfully")
        return True
    except subprocess.CalledProcessError as e:
        logger.error(f"  Rollout failed or timed out: {e.stderr}")
        return False


def verify_health(config: DeploymentConfig) -> bool:
    """Run post-deployment health check."""
    if not config.health_url:
        logger.info("  Skipping health check (no URL configured)")
        return True

    logger.info(f"Running health check: {config.health_url}")

    max_attempts = 10
    for attempt in range(1, max_attempts + 1):
        try:
            response = requests.get(config.health_url, timeout=10)
            if response.status_code == 200:
                logger.info(f"  ✅ Health check passed ({response.status_code})")
                return True
            logger.warning(f"  Health check returned {response.status_code} (attempt {attempt}/{max_attempts})")
        except requests.ConnectionError:
            logger.warning(f"  Health check connection failed (attempt {attempt}/{max_attempts})")

        if attempt < max_attempts:
            time.sleep(15)

    logger.error("  ❌ Health check failed after all attempts")
    return False


def rollback_deployment(config: DeploymentConfig) -> bool:
    """Roll back the deployment to the previous version."""
    logger.warning(f"Rolling back {config.app_name} in {config.namespace}...")
    try:
        run_kubectl([
            "rollout", "undo",
            f"deployment/{config.app_name}",
            "-n", config.namespace,
        ])
        logger.warning("  Rollback initiated")
        return True
    except subprocess.CalledProcessError as e:
        logger.error(f"  Rollback FAILED: {e.stderr}")
        return False


def deploy(config: DeploymentConfig) -> DeploymentResult:
    """Execute the full deployment workflow."""
    start_time = time.perf_counter()
    result = DeploymentResult(
        success=False,
        app_name=config.app_name,
        environment=config.environment,
        image_tag=config.image_tag,
        duration_seconds=0,
    )

    logger.info(f"{'=' * 60}")
    logger.info(f"  DEPLOYMENT: {config.app_name} → {config.environment}")
    logger.info(f"  Image tag : {config.image_tag}")
    logger.info(f"  Namespace : {config.namespace}")
    logger.info(f"  Dry run   : {config.dry_run}")
    logger.info(f"{'=' * 60}")

    # Step 1: Pre-deployment checks
    if not pre_deployment_checks(config):
        result.add_step("pre-checks", "failed", "Pre-deployment checks failed")
        result.error = "Pre-deployment checks failed"
        result.duration_seconds = time.perf_counter() - start_time
        return result
    result.add_step("pre-checks", "success")

    # Step 2: Update deployment
    if not update_deployment(config):
        result.add_step("update", "failed")
        result.error = "Failed to update deployment image"
        result.duration_seconds = time.perf_counter() - start_time
        return result
    result.add_step("update", "success")

    # Step 3: Wait for rollout
    if not wait_for_rollout(config):
        result.add_step("rollout", "failed")
        result.error = "Rollout failed or timed out"

        # Auto-rollback on rollout failure
        logger.error("Deployment failed — initiating automatic rollback...")
        rollback_deployment(config)
        result.add_step("rollback", "success")
        result.duration_seconds = time.perf_counter() - start_time
        return result
    result.add_step("rollout", "success")

    # Step 4: Health check
    if not verify_health(config):
        result.add_step("health-check", "failed")
        result.error = "Post-deployment health check failed"

        logger.error("Health check failed — initiating automatic rollback...")
        rollback_deployment(config)
        result.add_step("rollback", "success")
        result.duration_seconds = time.perf_counter() - start_time
        return result
    result.add_step("health-check", "success")

    result.success = True
    result.duration_seconds = time.perf_counter() - start_time
    logger.info(f"\n✅ Deployment complete in {result.duration_seconds:.1f}s")
    return result


def main() -> None:
    parser = argparse.ArgumentParser(description="Deployment orchestrator")
    parser.add_argument("--app",       required=True)
    parser.add_argument("--env",       required=True, choices=["dev", "staging", "production"])
    parser.add_argument("--tag",       required=True, help="Image tag")
    parser.add_argument("--namespace", default=None,  help="Kubernetes namespace")
    parser.add_argument("--health-url", default="")
    parser.add_argument("--timeout",   type=int, default=300)
    parser.add_argument("--dry-run",   action="store_true")
    parser.add_argument("-v", "--verbose", action="store_true")
    args = parser.parse_args()

    logging.basicConfig(
        level=logging.DEBUG if args.verbose else logging.INFO,
        format="%(asctime)s [%(levelname)s] %(message)s",
    )

    if args.env == "production" and not args.dry_run:
        print(f"\n⚠️  WARNING: You are deploying to PRODUCTION")
        print(f"   App: {args.app}, Tag: {args.tag}")
        confirm = input("\n   Type 'yes' to confirm: ").strip().lower()
        if confirm != "yes":
            print("Deployment cancelled.")
            sys.exit(0)

    config = DeploymentConfig(
        app_name=args.app,
        environment=args.env,
        image_tag=args.tag,
        namespace=args.namespace or args.env,
        health_url=args.health_url,
        timeout=args.timeout,
        dry_run=args.dry_run,
    )

    result = deploy(config)

    print(f"\n{'─' * 40}")
    print(f"  Deployment {'SUCCEEDED ✅' if result.success else 'FAILED ❌'}")
    print(f"  Duration: {result.duration_seconds:.1f}s")
    for step in result.steps:
        icon = "✅" if step["status"] == "success" else "❌"
        print(f"  {icon} {step['name']}")
    if result.error:
        print(f"\n  Error: {result.error}")
    print(f"{'─' * 40}\n")

    sys.exit(0 if result.success else 1)


if __name__ == "__main__":
    main()
```

---

---

## Lesson 8 — Infrastructure Automation — Lab Project

> **Goal:** Build a complete infrastructure status and management tool

---

### 8.1 Complete Infrastructure Automation Tool

```python
#!/usr/bin/env python3
"""
infra_check.py — Infrastructure health and status automation tool.

Checks: disk, memory, CPU, running services, open ports, log errors.
Generates: summary report with alert levels.

DevOps use case: Run from cron or monitoring system every 5 minutes.
"""
import argparse
import json
import logging
import os
import platform
import shutil
import socket
import subprocess
import sys
import time
from dataclasses import dataclass, field
from datetime import datetime
from pathlib import Path
from typing import List, Optional

logger = logging.getLogger(__name__)


@dataclass
class Check:
    name: str
    status: str        # "ok", "warning", "critical"
    message: str
    value: Optional[float] = None
    unit: str = ""


@dataclass
class InfraReport:
    hostname: str
    timestamp: str
    checks: List[Check] = field(default_factory=list)

    @property
    def ok_count(self) -> int:
        return sum(1 for c in self.checks if c.status == "ok")

    @property
    def warning_count(self) -> int:
        return sum(1 for c in self.checks if c.status == "warning")

    @property
    def critical_count(self) -> int:
        return sum(1 for c in self.checks if c.status == "critical")

    @property
    def overall_status(self) -> str:
        if self.critical_count > 0:
            return "critical"
        if self.warning_count > 0:
            return "warning"
        return "ok"


# ─── INDIVIDUAL CHECKS ───────────────────────────────────────────

def check_disk_usage(path: str = "/", warn_pct: int = 80, crit_pct: int = 90) -> Check:
    """Check disk usage on a filesystem."""
    usage = shutil.disk_usage(path)
    pct = (usage.used / usage.total) * 100
    free_gb = usage.free / (1024 ** 3)

    if pct >= crit_pct:
        status = "critical"
    elif pct >= warn_pct:
        status = "warning"
    else:
        status = "ok"

    return Check(
        name=f"disk:{path}",
        status=status,
        message=f"{pct:.1f}% used ({free_gb:.1f}GB free)",
        value=pct,
        unit="%",
    )


def check_memory_usage(warn_pct: int = 80, crit_pct: int = 90) -> Check:
    """Check memory usage using /proc/meminfo (Linux only)."""
    if platform.system() != "Linux":
        # Use a cross-platform fallback
        try:
            import psutil
            vm = psutil.virtual_memory()
            pct = vm.percent
        except ImportError:
            return Check("memory", "ok", "psutil not available — skipped")
    else:
        with open("/proc/meminfo") as f:
            meminfo = {}
            for line in f:
                parts = line.split()
                meminfo[parts[0].rstrip(":")] = int(parts[1])

        total = meminfo.get("MemTotal", 0)
        available = meminfo.get("MemAvailable", 0)
        pct = ((total - available) / total) * 100 if total > 0 else 0

    if pct >= crit_pct:
        status = "critical"
    elif pct >= warn_pct:
        status = "warning"
    else:
        status = "ok"

    return Check("memory", status, f"{pct:.1f}% used", pct, "%")


def check_service_running(service_name: str) -> Check:
    """Check if a systemd service is running."""
    try:
        result = subprocess.run(
            ["systemctl", "is-active", service_name],
            capture_output=True, text=True,
        )
        active = result.stdout.strip() == "active"
        return Check(
            f"service:{service_name}",
            "ok" if active else "critical",
            "running" if active else "NOT RUNNING",
        )
    except FileNotFoundError:
        return Check(f"service:{service_name}", "ok", "systemctl not available — skipped")


def check_port_open(host: str, port: int, timeout: float = 3.0) -> Check:
    """Check if a TCP port is reachable."""
    try:
        with socket.create_connection((host, port), timeout=timeout):
            return Check(f"port:{host}:{port}", "ok", f"reachable")
    except (socket.timeout, ConnectionRefusedError, OSError) as e:
        return Check(f"port:{host}:{port}", "critical", f"unreachable: {e}")


def check_log_errors(log_file: str, error_pattern: str = "ERROR", max_errors: int = 5) -> Check:
    """Check for recent errors in a log file."""
    path = Path(log_file)
    if not path.exists():
        return Check(f"log:{log_file}", "ok", "file not found — skipped")

    # Read last 1000 lines (tail)
    try:
        result = subprocess.run(
            ["tail", "-n", "1000", log_file],
            capture_output=True, text=True, timeout=5,
        )
        error_count = result.stdout.count(error_pattern)
    except (subprocess.TimeoutExpired, FileNotFoundError):
        return Check(f"log:{log_file}", "ok", "tail not available — skipped")

    if error_count >= max_errors:
        status = "critical"
    elif error_count > 0:
        status = "warning"
    else:
        status = "ok"

    return Check(
        f"log:{path.name}",
        status,
        f"{error_count} '{error_pattern}' entries in last 1000 lines",
        error_count,
    )


# ─── REPORT GENERATION ────────────────────────────────────────────

def generate_report(config: dict) -> InfraReport:
    """Run all configured checks and return a report."""
    report = InfraReport(
        hostname=socket.gethostname(),
        timestamp=datetime.utcnow().isoformat() + "Z",
    )

    # Disk checks
    for disk_path in config.get("disks", ["/"]):
        report.checks.append(check_disk_usage(disk_path))

    # Memory
    report.checks.append(check_memory_usage())

    # Service checks
    for service in config.get("services", []):
        report.checks.append(check_service_running(service))

    # Port checks
    for port_check in config.get("ports", []):
        host = port_check.get("host", "localhost")
        port = port_check["port"]
        report.checks.append(check_port_open(host, port))

    # Log checks
    for log_check in config.get("logs", []):
        report.checks.append(check_log_errors(
            log_check["file"],
            log_check.get("pattern", "ERROR"),
            log_check.get("max_errors", 5),
        ))

    return report


def print_report(report: InfraReport) -> None:
    """Print a human-readable report."""
    status_icons = {"ok": "✅", "warning": "⚠️ ", "critical": "❌"}
    overall_icon  = status_icons[report.overall_status]

    print(f"\n{'─' * 60}")
    print(f"  Infrastructure Health Check — {report.hostname}")
    print(f"  {report.timestamp}")
    print(f"{'─' * 60}")

    for check in report.checks:
        icon = status_icons.get(check.status, "?")
        value_str = f" ({check.value:.1f}{check.unit})" if check.value is not None else ""
        print(f"  {icon} {check.name:<30} {check.message}{value_str}")

    print(f"{'─' * 60}")
    print(f"  {overall_icon} Overall: {report.overall_status.upper()}")
    print(f"     OK: {report.ok_count}  WARNING: {report.warning_count}  CRITICAL: {report.critical_count}")
    print(f"{'─' * 60}\n")


def main() -> None:
    parser = argparse.ArgumentParser(description="Infrastructure health check")
    parser.add_argument("--config", type=Path, help="YAML config file")
    parser.add_argument("--format", choices=["text", "json"], default="text")
    parser.add_argument("--fail-on-warning", action="store_true",
                        help="Exit code 1 on warnings too (not just critical)")
    args = parser.parse_args()

    logging.basicConfig(level=logging.WARNING, format="%(levelname)s: %(message)s")

    # Default config (override with --config file)
    config = {
        "disks": ["/"],
        "services": [],
        "ports": [{"host": "localhost", "port": 80}],
        "logs": [],
    }

    if args.config and args.config.exists():
        import yaml
        with open(args.config) as f:
            config.update(yaml.safe_load(f))

    report = generate_report(config)

    if args.format == "json":
        output = {
            "hostname": report.hostname,
            "timestamp": report.timestamp,
            "overall": report.overall_status,
            "checks": [
                {"name": c.name, "status": c.status, "message": c.message}
                for c in report.checks
            ],
        }
        print(json.dumps(output, indent=2))
    else:
        print_report(report)

    if report.critical_count > 0:
        sys.exit(2)
    if args.fail_on_warning and report.warning_count > 0:
        sys.exit(1)
    sys.exit(0)


if __name__ == "__main__":
    main()
```

```bash
pip install pyyaml

# Run with default config
python infra_check.py

# Run with JSON output (pipe to monitoring system)
python infra_check.py --format json

# Create a config file
cat > infra_config.yml << 'EOF'
disks:
  - "/"
  - "/tmp"
services:
  - nginx
  - postgresql
ports:
  - host: localhost
    port: 5432
  - host: localhost
    port: 80
logs:
  - file: "/var/log/nginx/error.log"
    pattern: "ERROR"
    max_errors: 3
EOF

python infra_check.py --config infra_config.yml
```


---

---

# PHASE 2 — BASH SCRIPTING FUNDAMENTALS

---

## Lesson 9 — Bash Shell Basics and Script Execution

> **Goal:** Write reliable Bash scripts that work correctly and fail safely

---

### 9.1 The Shebang and Script Execution

```bash
#!/usr/bin/env bash
# ^^^ ALWAYS use this shebang — not #!/bin/bash
# Why: /usr/bin/env finds bash in PATH, works on any system
# /bin/bash hardcodes the location which varies across systems

# Make executable and run
chmod +x myscript.sh
./myscript.sh

# Or explicitly:
bash myscript.sh

# Debug mode — prints every command before running it
bash -x myscript.sh

# Check for syntax errors without running
bash -n myscript.sh
```

---

### 9.2 The Safety Trio — Always Start Scripts With These

```bash
#!/usr/bin/env bash
set -euo pipefail
# ^^^
# -e  exit immediately if any command fails (non-zero exit code)
# -u  treat undefined variables as errors (catch typos)
# -o pipefail  pipe fails if ANY command in it fails (not just the last)

# WHY THIS MATTERS — without set -e:
cp important_file.txt /nonexistent/path/    # Fails silently
rm -rf /   # This would run even though cp failed — DISASTER

# WITH set -e: the script exits at the cp failure

# -u example:
ENVIRONMENT="production"
echo $ENVIRONMET    # Typo! Without -u: prints empty string silently
                    # With -u: "ENVIRONMET: unbound variable" — error caught!

# pipefail example:
cat /nonexistent/file | grep "pattern"    # Without -o pipefail: exit 0 (grep succeeded)
                                           # With -o pipefail: exit 1 (cat failed)

# Disable for specific commands where failure is acceptable:
set +e                        # Turn off errexit temporarily
grep "pattern" file.txt       # OK if grep finds nothing (exit 1 = no match)
GREP_RESULT=$?                # Save exit code
set -e                        # Turn errexit back on
```

---

### 9.3 Bash Script Structure — Professional Template

```bash
#!/usr/bin/env bash
# =============================================================================
# script_name.sh — One-line description
#
# USAGE:
#   ./script_name.sh [OPTIONS] <argument>
#
# OPTIONS:
#   -e, --env ENV         Target environment (dev|staging|production)
#   -v, --verbose         Enable verbose output
#   -n, --dry-run         Preview changes without executing
#   -h, --help            Show this help
#
# EXAMPLES:
#   ./script_name.sh --env production
#   ./script_name.sh --env staging --dry-run
#
# REQUIREMENTS:
#   - kubectl >= 1.26
#   - AWS CLI >= 2.0
#   - jq >= 1.6
# =============================================================================
set -euo pipefail

# ─── CONSTANTS ──────────────────────────────────────────────────────────────
readonly SCRIPT_NAME="$(basename "$0")"
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly LOG_FILE="/tmp/${SCRIPT_NAME%.sh}.log"
readonly TIMESTAMP="$(date +%Y%m%d_%H%M%S)"

# ─── COLOURS ────────────────────────────────────────────────────────────────
readonly RED='\033[0;31m'
readonly YELLOW='\033[1;33m'
readonly GREEN='\033[0;32m'
readonly BLUE='\033[0;34m'
readonly NC='\033[0m'   # No Colour

# ─── LOGGING ────────────────────────────────────────────────────────────────
log()     { echo -e "${GREEN}[$(date +'%Y-%m-%d %H:%M:%S')] INFO${NC}  $*" | tee -a "$LOG_FILE"; }
warn()    { echo -e "${YELLOW}[$(date +'%Y-%m-%d %H:%M:%S')] WARN${NC}  $*" | tee -a "$LOG_FILE" >&2; }
error()   { echo -e "${RED}[$(date +'%Y-%m-%d %H:%M:%S')] ERROR${NC} $*" | tee -a "$LOG_FILE" >&2; }
debug()   { [[ "${VERBOSE:-false}" == "true" ]] && echo -e "${BLUE}[$(date +'%Y-%m-%d %H:%M:%S')] DEBUG${NC} $*" | tee -a "$LOG_FILE"; }
success() { echo -e "${GREEN}[$(date +'%Y-%m-%d %H:%M:%S')] ✅${NC}    $*" | tee -a "$LOG_FILE"; }

# ─── CLEANUP ────────────────────────────────────────────────────────────────
cleanup() {
    local exit_code=$?
    debug "Cleanup called (exit code: ${exit_code})"
    # Add cleanup tasks here: remove temp files, release locks, etc.
    rm -f /tmp/${SCRIPT_NAME}.lock 2>/dev/null || true
    exit "$exit_code"
}
trap cleanup EXIT
trap 'error "Script interrupted"; exit 130' INT TERM

# ─── USAGE ──────────────────────────────────────────────────────────────────
usage() {
    grep '^#' "$0" | grep -v '#!/' | sed 's/^# \?//'
}

# ─── DEPENDENCY CHECK ───────────────────────────────────────────────────────
check_dependencies() {
    local -a required=("kubectl" "jq" "curl")
    local missing=()

    for cmd in "${required[@]}"; do
        if ! command -v "$cmd" &>/dev/null; then
            missing+=("$cmd")
        fi
    done

    if [[ ${#missing[@]} -gt 0 ]]; then
        error "Missing required tools: ${missing[*]}"
        error "Install with: apt-get install ${missing[*]}"
        exit 1
    fi
}

# ─── ARGUMENT PARSING ───────────────────────────────────────────────────────
ENVIRONMENT=""
VERBOSE="false"
DRY_RUN="false"

parse_args() {
    while [[ $# -gt 0 ]]; do
        case "$1" in
            -e|--env)
                ENVIRONMENT="$2"
                shift 2
                ;;
            -v|--verbose)
                VERBOSE="true"
                shift
                ;;
            -n|--dry-run)
                DRY_RUN="true"
                shift
                ;;
            -h|--help)
                usage
                exit 0
                ;;
            *)
                error "Unknown option: $1"
                usage
                exit 2
                ;;
        esac
    done

    # Validation
    if [[ -z "$ENVIRONMENT" ]]; then
        error "Required: --env <dev|staging|production>"
        exit 2
    fi

    if [[ "$ENVIRONMENT" != "dev" && "$ENVIRONMENT" != "staging" && "$ENVIRONMENT" != "production" ]]; then
        error "Invalid environment: '$ENVIRONMENT' (must be dev, staging, or production)"
        exit 2
    fi
}

# ─── MAIN ───────────────────────────────────────────────────────────────────
main() {
    parse_args "$@"
    check_dependencies

    log "Starting ${SCRIPT_NAME}"
    log "Environment : ${ENVIRONMENT}"
    log "Dry run     : ${DRY_RUN}"

    if [[ "$DRY_RUN" == "true" ]]; then
        warn "DRY RUN MODE — no changes will be made"
    fi

    # Your main logic here
    success "${SCRIPT_NAME} completed successfully"
}

main "$@"
```

---

---

## Lesson 10 — Variables and Environment Variables

> **Goal:** Master Bash variable handling — the source of most Bash bugs

---

### 10.1 Variable Fundamentals

```bash
#!/usr/bin/env bash
set -euo pipefail

# Assignment — NO spaces around =
NAME="production"
PORT=8080
IS_ACTIVE=true     # Note: just a string, not a real boolean in Bash

# Access with ${}  (always use braces — prevents ambiguity)
echo "${NAME}"
echo "${PORT}"
echo "Service: ${NAME}-api on port ${PORT}"

# ─── QUOTING RULES — the #1 source of Bash bugs ──────────────────
FILE="my file with spaces.txt"

# WRONG — word splitting breaks this
ls $FILE       # Bash sees: ls my file with spaces.txt (4 arguments!)

# CORRECT — always quote variable expansions
ls "${FILE}"   # Bash sees: ls "my file with spaces.txt" (1 argument)

# ALWAYS quote variables in: if, [[ ]], loops, commands
if [[ "${ENVIRONMENT}" == "production" ]]; then
    echo "Production mode"
fi

# ─── SPECIAL VARIABLES ───────────────────────────────────────────
echo "$0"      # Script name
echo "$1"      # First argument
echo "$2"      # Second argument
echo "$@"      # All arguments (as separate words — use this)
echo "$*"      # All arguments (as one word — avoid)
echo "$#"      # Number of arguments
echo "$$"      # Current process ID (PID)
echo "$?"      # Exit code of last command
echo "$!"      # PID of last background process

# ─── VARIABLE OPERATIONS ─────────────────────────────────────────
VAR="Hello, World!"

# Length
echo "${#VAR}"              # 13

# Uppercase / lowercase (Bash 4+)
echo "${VAR,,}"             # hello, world!
echo "${VAR^^}"             # HELLO, WORLD!

# Substring: ${VAR:offset:length}
echo "${VAR:0:5}"           # Hello
echo "${VAR:7}"             # World!
echo "${VAR: -6}"           # World!  (negative offset)

# Remove prefix/suffix
FILE="application.log.gz"
echo "${FILE%.gz}"          # application.log  (remove shortest suffix)
echo "${FILE%%.*}"          # application      (remove longest suffix)
echo "${FILE#*.}"           # log.gz           (remove shortest prefix)
echo "${FILE##*.}"          # gz               (remove longest prefix)

# Search and replace
PATH_VAR="/home/alice/docs/file.txt"
echo "${PATH_VAR/alice/bob}"    # /home/bob/docs/file.txt  (first match)
echo "${PATH_VAR//o/0}"        # /h0me/alice/d0cs/file.txt (all matches)

# ─── DEFAULT VALUES ──────────────────────────────────────────────
# Use default if unset or empty
echo "${ENVIRONMENT:-dev}"                # dev (if ENVIRONMENT unset)

# Use default only if unset (not empty)
echo "${ENVIRONMENT-dev}"

# Assign default if unset
: "${LOG_LEVEL:=INFO}"   # Sets LOG_LEVEL to INFO if not already set
echo "${LOG_LEVEL}"      # INFO

# Error if unset
echo "${REQUIRED_VAR:?'REQUIRED_VAR must be set'}"
```

---

### 10.2 Arrays

```bash
#!/usr/bin/env bash
set -euo pipefail

# ─── INDEXED ARRAYS ──────────────────────────────────────────────
SERVICES=("nginx" "postgresql" "redis" "celery")

# Access elements
echo "${SERVICES[0]}"        # nginx (first element)
echo "${SERVICES[-1]}"       # celery (last element)
echo "${SERVICES[@]}"        # all elements
echo "${#SERVICES[@]}"       # 4 (number of elements)

# Iterate
for service in "${SERVICES[@]}"; do
    echo "Checking: ${service}"
done

# Add elements
SERVICES+=("kafka")

# Slice: ${array[@]:start:count}
echo "${SERVICES[@]:1:2}"    # postgresql redis

# ─── ASSOCIATIVE ARRAYS (dictionaries) ───────────────────────────
declare -A PORTS
PORTS["nginx"]=80
PORTS["postgresql"]=5432
PORTS["redis"]=6379
PORTS["api"]=3000

echo "${PORTS["nginx"]}"     # 80

# Iterate keys and values
for service in "${!PORTS[@]}"; do
    echo "${service}: ${PORTS[$service]}"
done

# Check if key exists
if [[ -v PORTS["nginx"] ]]; then
    echo "nginx port is defined"
fi

# ─── PRACTICAL ARRAY USAGE ───────────────────────────────────────
# Collect output into array
mapfile -t LOG_FILES < <(find /var/log -name "*.log" -type f)
echo "Found ${#LOG_FILES[@]} log files"

# Split a string into array
CSV="alice,bob,charlie"
IFS=',' read -ra USERS <<< "${CSV}"
for user in "${USERS[@]}"; do
    echo "User: ${user}"
done
```

---

---

## Lesson 11 — Conditionals and Exit Codes

> **Goal:** Write conditional logic that handles real-world edge cases

---

### 11.1 Test Conditions — [[ ]] vs [ ]

```bash
#!/usr/bin/env bash
set -euo pipefail

# ALWAYS use [[ ]] not [ ] in Bash scripts
# [[ ]] is a Bash builtin with better behaviour (no word splitting, regex support)
# [ ] is POSIX sh — less capable, more surprising

FILE="/etc/hosts"
VALUE="hello"
NUM=42

# ─── FILE TESTS ──────────────────────────────────────────────────
[[ -e "$FILE" ]]  && echo "Exists"
[[ -f "$FILE" ]]  && echo "Is a regular file"
[[ -d "$FILE" ]]  && echo "Is a directory"
[[ -r "$FILE" ]]  && echo "Readable"
[[ -w "$FILE" ]]  && echo "Writable"
[[ -x "$FILE" ]]  && echo "Executable"
[[ -s "$FILE" ]]  && echo "Size > 0 (non-empty)"
[[ -L "$FILE" ]]  && echo "Is a symlink"
[[ -z "$VALUE" ]] && echo "String is empty"
[[ -n "$VALUE" ]] && echo "String is non-empty"

# ─── STRING TESTS ────────────────────────────────────────────────
[[ "$VALUE" == "hello" ]]   && echo "Equal"
[[ "$VALUE" != "world" ]]   && echo "Not equal"
[[ "$VALUE" < "world" ]]    && echo "Alphabetically before"
[[ "$VALUE" =~ ^hel.* ]]   && echo "Matches regex"   # [[ ]] only!

# ─── NUMERIC TESTS ───────────────────────────────────────────────
[[ $NUM -eq 42 ]] && echo "Equal to 42"
[[ $NUM -ne 0  ]] && echo "Not zero"
[[ $NUM -gt 10 ]] && echo "Greater than 10"
[[ $NUM -lt 100 ]] && echo "Less than 100"
[[ $NUM -ge 42 ]] && echo "Greater than or equal to 42"
[[ $NUM -le 50 ]] && echo "Less than or equal to 50"

# ─── COMPOUND CONDITIONS ─────────────────────────────────────────
ENV="production"
REPLICAS=5

if [[ "$ENV" == "production" && $REPLICAS -ge 3 ]]; then
    echo "Production with adequate replicas"
fi

if [[ "$ENV" == "staging" || "$ENV" == "dev" ]]; then
    echo "Non-production environment"
fi

if [[ ! -f "/etc/config.yml" ]]; then
    echo "Config file missing!"
fi
```

---

### 11.2 Practical Conditionals in DevOps

```bash
#!/usr/bin/env bash
set -euo pipefail

# ─── CHECKING COMMANDS EXIST ─────────────────────────────────────
if ! command -v kubectl &>/dev/null; then
    echo "Error: kubectl is not installed" >&2
    exit 1
fi

# ─── CHECKING ENVIRONMENT VARIABLES ─────────────────────────────
for var in AWS_REGION AWS_ACCOUNT_ID CLUSTER_NAME; do
    if [[ -z "${!var:-}" ]]; then   # Indirect variable reference
        echo "Error: Required variable $var is not set" >&2
        exit 1
    fi
done

# ─── COMPARING VERSIONS ──────────────────────────────────────────
KUBECTL_VERSION=$(kubectl version --client -o json | jq -r '.clientVersion.minor')
REQUIRED_VERSION=26

if [[ $KUBECTL_VERSION -lt $REQUIRED_VERSION ]]; then
    echo "Error: kubectl >= 1.${REQUIRED_VERSION} required (got 1.${KUBECTL_VERSION})" >&2
    exit 1
fi

# ─── PRODUCTION SAFETY CHECKS ────────────────────────────────────
ENVIRONMENT="${1:-}"

if [[ "$ENVIRONMENT" == "production" ]]; then
    echo "⚠️  WARNING: You are about to modify PRODUCTION"
    echo "   This will affect live users."
    read -r -p "   Type 'yes' to confirm: " CONFIRM

    if [[ "$CONFIRM" != "yes" ]]; then
        echo "Cancelled."
        exit 0
    fi
fi

# ─── CASE STATEMENT — cleaner than if/elif for multiple values ────
case "${ENVIRONMENT:-}" in
    dev|development)
        NAMESPACE="development"
        REPLICAS=1
        LOG_LEVEL="debug"
        ;;
    staging|stage)
        NAMESPACE="staging"
        REPLICAS=2
        LOG_LEVEL="info"
        ;;
    production|prod)
        NAMESPACE="production"
        REPLICAS=5
        LOG_LEVEL="warn"
        ;;
    "")
        echo "Error: ENVIRONMENT not set" >&2
        exit 1
        ;;
    *)
        echo "Error: Unknown environment: '${ENVIRONMENT}'" >&2
        exit 1
        ;;
esac

echo "Deploying to ${NAMESPACE} with ${REPLICAS} replicas"
```

---

---

## Lesson 12 — Loops and Arrays

> **Goal:** Iterate over files, services, hosts, and command outputs

---

### 12.1 For Loops — Every Pattern You Need

```bash
#!/usr/bin/env bash
set -euo pipefail

# ─── ITERATE OVER LIST ───────────────────────────────────────────
SERVICES=("nginx" "postgresql" "redis")

for service in "${SERVICES[@]}"; do
    echo "Service: $service"
done

# ─── ITERATE OVER RANGE ──────────────────────────────────────────
for i in {1..5}; do
    echo "Attempt $i"
done

for i in $(seq 1 5); do   # seq allows variables
    echo "Server: web-${i}"
done

# ─── ITERATE OVER FILES ──────────────────────────────────────────
# WRONG — breaks on filenames with spaces
for f in $(ls /etc/*.conf); do
    echo "$f"
done

# CORRECT — use glob directly
for config_file in /etc/*.conf; do
    [[ -f "$config_file" ]] || continue   # Skip if no match
    echo "Config: $config_file"
done

# ─── ITERATE OVER COMMAND OUTPUT ─────────────────────────────────
# Read lines from command output safely
while IFS= read -r line; do
    echo "Pod: $line"
done < <(kubectl get pods -n production --no-headers -o custom-columns=":metadata.name")

# ─── ITERATE OVER CSV ────────────────────────────────────────────
while IFS=',' read -r hostname ip role; do
    echo "Host: $hostname ($ip) — Role: $role"
done < servers.csv

# ─── C-STYLE FOR LOOP ────────────────────────────────────────────
for ((i=0; i<5; i++)); do
    echo "Index: $i"
done
```

---

### 12.2 While and Until Loops

```bash
#!/usr/bin/env bash
set -euo pipefail

# ─── WAIT FOR SERVICE TO BE READY ────────────────────────────────
wait_for_service() {
    local service="$1"
    local url="$2"
    local max_attempts="${3:-30}"
    local delay="${4:-5}"
    local attempt=1

    echo "Waiting for ${service}..."

    while [[ $attempt -le $max_attempts ]]; do
        if curl -sf "${url}/health" &>/dev/null; then
            echo "✅ ${service} is ready (attempt ${attempt}/${max_attempts})"
            return 0
        fi
        echo "  Attempt ${attempt}/${max_attempts} — waiting ${delay}s..."
        sleep "$delay"
        ((attempt++))
    done

    echo "❌ ${service} did not become ready after ${max_attempts} attempts" >&2
    return 1
}

# Usage
wait_for_service "API" "http://api:3000" 20 3

# ─── RETRY WITH EXPONENTIAL BACKOFF ──────────────────────────────
retry() {
    local max_attempts="$1"
    shift
    local cmd=("$@")
    local attempt=1

    while [[ $attempt -le $max_attempts ]]; do
        if "${cmd[@]}"; then
            return 0
        fi

        if [[ $attempt -lt $max_attempts ]]; then
            local delay=$((2 ** attempt))
            echo "Command failed — retry ${attempt}/${max_attempts} in ${delay}s" >&2
            sleep "$delay"
        fi
        ((attempt++))
    done

    echo "Command failed after ${max_attempts} attempts: ${cmd[*]}" >&2
    return 1
}

# Usage
retry 3 kubectl apply -f deployment.yml

# ─── PROCESS LINE BY LINE ────────────────────────────────────────
process_log_file() {
    local log_file="$1"
    local error_count=0

    while IFS= read -r line; do
        if [[ "$line" =~ ERROR ]]; then
            echo "ERROR FOUND: $line" >&2
            ((error_count++))
        fi
    done < "$log_file"

    return $((error_count > 0 ? 1 : 0))
}
```

---

---

## Lesson 13 — Functions and Command Substitution

> **Goal:** Write reusable functions and capture command output

---

### 13.1 Functions — Best Practices

```bash
#!/usr/bin/env bash
set -euo pipefail

# ─── FUNCTION DEFINITION ─────────────────────────────────────────
# Prefer: name() { } over function name { }
deploy_app() {
    # Local variables — ALWAYS use local inside functions!
    local app_name="$1"
    local version="$2"
    local namespace="${3:-default}"    # Default value

    echo "Deploying ${app_name}:${version} to ${namespace}"

    # Return values: use echo + command substitution to capture
    # Return codes: use return 0/1 for success/failure
    if kubectl set image "deployment/${app_name}" \
        "${app_name}=myregistry/${app_name}:${version}" \
        -n "${namespace}"; then
        return 0    # Success
    else
        return 1    # Failure
    fi
}

# ─── CAPTURING RETURN VALUES ─────────────────────────────────────
get_pod_count() {
    local namespace="$1"
    local app="$2"
    # Echo the result — caller captures with $()
    kubectl get pods -n "$namespace" -l "app=$app" --no-headers 2>/dev/null | wc -l | tr -d ' '
}

POD_COUNT=$(get_pod_count "production" "api")
echo "Running pods: ${POD_COUNT}"

# ─── ERROR HANDLING IN FUNCTIONS ─────────────────────────────────
check_and_deploy() {
    local app="$1"
    local version="$2"

    # Validate inputs
    if [[ -z "$app" ]]; then
        echo "Error: app name required" >&2
        return 1
    fi

    if [[ ! "$version" =~ ^v[0-9]+\.[0-9]+\.[0-9]+ ]]; then
        echo "Error: version must be in format vX.Y.Z (got: $version)" >&2
        return 1
    fi

    deploy_app "$app" "$version" "production" || {
        echo "Deployment failed for ${app}:${version}" >&2
        return 1
    }

    return 0
}
```

---

### 13.2 Command Substitution

```bash
#!/usr/bin/env bash
set -euo pipefail

# $() syntax — ALWAYS use this, not backticks `cmd`
# $() is nestable and easier to read

CURRENT_DATE=$(date +%Y-%m-%d)
GIT_SHA=$(git rev-parse --short HEAD)
HOSTNAME=$(hostname -f)
POD_COUNT=$(kubectl get pods -n production --no-headers | wc -l | tr -d ' ')

echo "Deploy ${GIT_SHA} at ${CURRENT_DATE} on ${HOSTNAME}"

# Multi-line command substitution
RUNNING_PODS=$(kubectl get pods -n production \
    --field-selector=status.phase=Running \
    --no-headers \
    -o custom-columns=":metadata.name")

echo "Running pods:"
echo "$RUNNING_PODS"

# Nested substitution
LATEST_IMAGE=$(docker inspect \
    "$(kubectl get deployment api -n production -o jsonpath='{.spec.template.spec.containers[0].image}')" \
    --format='{{.Id}}' 2>/dev/null || echo "not-found")

# Process substitution — treat command output as a file
diff <(kubectl get pods -n staging --no-headers) \
     <(kubectl get pods -n production --no-headers)

while IFS= read -r line; do
    echo "Processing: $line"
done < <(kubectl get pods -n production --no-headers)
```

---

---

# PHASE 3 — DEVOPS BASH AUTOMATION

---

## Lesson 14 — System Monitoring Scripts

> **Goal:** Build monitoring scripts that run continuously and alert on problems

---

### 14.1 Complete System Monitor

```bash
#!/usr/bin/env bash
# =============================================================================
# system_monitor.sh — Continuous system resource monitoring
#
# USAGE:
#   ./system_monitor.sh [--interval 60] [--slack-webhook URL] [--log /var/log/monitor.log]
#
# THRESHOLDS (customisable via environment):
#   DISK_WARN_PCT=80    DISK_CRIT_PCT=90
#   MEM_WARN_PCT=80     MEM_CRIT_PCT=90
#   CPU_WARN_PCT=80     CPU_CRIT_PCT=95
#   LOAD_WARN=4         LOAD_CRIT=8
# =============================================================================
set -euo pipefail

readonly SCRIPT_NAME="$(basename "$0")"

# ─── DEFAULTS (override with environment variables) ──────────────
: "${DISK_WARN_PCT:=80}"
: "${DISK_CRIT_PCT:=90}"
: "${MEM_WARN_PCT:=80}"
: "${MEM_CRIT_PCT:=90}"
: "${CPU_WARN_PCT:=80}"
: "${CPU_CRIT_PCT:=95}"
: "${LOAD_WARN:=4}"
: "${LOAD_CRIT:=8}"
: "${CHECK_INTERVAL:=60}"
: "${SLACK_WEBHOOK:=}"
: "${LOG_FILE:=/var/log/system_monitor.log}"

# ─── COLOURS ────────────────────────────────────────────────────
RED='\033[0;31m'; YELLOW='\033[1;33m'; GREEN='\033[0;32m'; NC='\033[0m'

log() {
    local level="$1"; shift
    local msg="$*"
    local ts; ts="$(date +'%Y-%m-%d %H:%M:%S')"
    echo "[${ts}] [${level}] ${msg}" | tee -a "${LOG_FILE}"
}

send_slack_alert() {
    local level="$1"
    local message="$2"
    [[ -z "${SLACK_WEBHOOK}" ]] && return 0

    local icon="ℹ️"
    [[ "$level" == "WARNING"  ]] && icon="⚠️"
    [[ "$level" == "CRITICAL" ]] && icon="🚨"

    local hostname; hostname="$(hostname -s)"
    local payload="{\"text\": \"${icon} *${level}* on \`${hostname}\`: ${message}\"}"

    curl -sf -X POST "${SLACK_WEBHOOK}" \
        -H 'Content-type: application/json' \
        --data "$payload" &>/dev/null || true
}

# ─── CHECK FUNCTIONS ─────────────────────────────────────────────
check_disk() {
    local status="OK"
    local alerts=()

    while IFS= read -r line; do
        local pct filesystem
        pct=$(echo "$line" | awk '{print $5}' | tr -d '%')
        filesystem=$(echo "$line" | awk '{print $6}')

        if [[ $pct -ge $DISK_CRIT_PCT ]]; then
            alerts+=("CRITICAL: ${filesystem} at ${pct}% (threshold: ${DISK_CRIT_PCT}%)")
            status="CRITICAL"
        elif [[ $pct -ge $DISK_WARN_PCT ]]; then
            alerts+=("WARNING: ${filesystem} at ${pct}% (threshold: ${DISK_WARN_PCT}%)")
            [[ "$status" != "CRITICAL" ]] && status="WARNING"
        fi
    done < <(df -h | grep -E '^/dev/' | grep -v tmpfs)

    for alert in "${alerts[@]}"; do
        log "$status" "DISK: $alert"
        send_slack_alert "$status" "$alert"
    done

    echo "$status"
}

check_memory() {
    if [[ ! -f /proc/meminfo ]]; then
        echo "OK"
        return
    fi

    local total available
    total=$(awk '/^MemTotal/ {print $2}' /proc/meminfo)
    available=$(awk '/^MemAvailable/ {print $2}' /proc/meminfo)

    local used=$((total - available))
    local pct=$((used * 100 / total))

    if [[ $pct -ge $MEM_CRIT_PCT ]]; then
        log "CRITICAL" "MEMORY: ${pct}% used (threshold: ${MEM_CRIT_PCT}%)"
        send_slack_alert "CRITICAL" "Memory at ${pct}%"
        echo "CRITICAL"
    elif [[ $pct -ge $MEM_WARN_PCT ]]; then
        log "WARNING" "MEMORY: ${pct}% used (threshold: ${MEM_WARN_PCT}%)"
        send_slack_alert "WARNING" "Memory at ${pct}%"
        echo "WARNING"
    else
        echo "OK"
    fi
}

check_load() {
    local load_1min
    load_1min=$(awk '{print $1}' /proc/loadavg | cut -d. -f1)

    if [[ $load_1min -ge $LOAD_CRIT ]]; then
        log "CRITICAL" "LOAD: ${load_1min} (threshold: ${LOAD_CRIT})"
        send_slack_alert "CRITICAL" "System load at ${load_1min}"
        echo "CRITICAL"
    elif [[ $load_1min -ge $LOAD_WARN ]]; then
        log "WARNING" "LOAD: ${load_1min} (threshold: ${LOAD_WARN})"
        echo "WARNING"
    else
        echo "OK"
    fi
}

check_services() {
    local services=("nginx" "postgresql" "redis")
    local failed=()

    for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            log "OK" "SERVICE: ${service} is running"
        else
            failed+=("$service")
            log "CRITICAL" "SERVICE: ${service} is NOT running"
        fi
    done

    if [[ ${#failed[@]} -gt 0 ]]; then
        send_slack_alert "CRITICAL" "Services down: ${failed[*]}"
        echo "CRITICAL"
    else
        echo "OK"
    fi
}

# ─── MAIN MONITORING LOOP ─────────────────────────────────────────
main() {
    log "INFO" "System monitor started (interval: ${CHECK_INTERVAL}s)"

    while true; do
        log "INFO" "─── Health check at $(date +'%H:%M:%S') ───"

        local overall_status="OK"

        disk_status=$(check_disk)
        mem_status=$(check_memory)
        load_status=$(check_load)
        # service_status=$(check_services)   # Uncomment when systemd available

        for status in "$disk_status" "$mem_status" "$load_status"; do
            if [[ "$status" == "CRITICAL" ]]; then
                overall_status="CRITICAL"
                break
            elif [[ "$status" == "WARNING" && "$overall_status" != "CRITICAL" ]]; then
                overall_status="WARNING"
            fi
        done

        log "INFO" "Overall: ${overall_status}"
        sleep "${CHECK_INTERVAL}"
    done
}

main "$@"
```

---

---

## Lesson 15 — Log Analysis Scripts

> **Goal:** Extract insights from log files at scale

---

### 15.1 Production Log Analyzer

```bash
#!/usr/bin/env bash
# =============================================================================
# log_analyzer.sh — Analyse application and web server logs
#
# USAGE:
#   ./log_analyzer.sh --file /var/log/nginx/access.log --period 1h
#   ./log_analyzer.sh --file /var/log/app.log --errors-only
# =============================================================================
set -euo pipefail

# ─── NGINX ACCESS LOG ANALYSIS ───────────────────────────────────
analyze_nginx() {
    local log_file="$1"
    local lines="${2:-10000}"   # Last N lines to analyze

    echo "═══════════════════════════════════════════════════════"
    echo "  Nginx Access Log Analysis — $(date +'%Y-%m-%d %H:%M:%S')"
    echo "  File: ${log_file}"
    echo "═══════════════════════════════════════════════════════"

    # Total requests
    local total
    total=$(tail -n "$lines" "$log_file" | wc -l)
    echo ""
    echo "  Total requests (last ${lines} lines): ${total}"

    # Status code distribution
    echo ""
    echo "  Status Codes:"
    echo "  ─────────────────────────────────"
    tail -n "$lines" "$log_file" \
        | awk '{print $9}' \
        | sort | uniq -c | sort -rn \
        | while read -r count code; do
            printf "  %8d  %s\n" "$count" "$code"
        done

    # Error rate
    local errors_5xx
    errors_5xx=$(tail -n "$lines" "$log_file" | awk '$9 ~ /^5/' | wc -l)
    local error_rate=0
    [[ $total -gt 0 ]] && error_rate=$((errors_5xx * 100 / total))
    echo ""
    echo "  5xx Error Rate: ${error_rate}% (${errors_5xx}/${total})"

    # Top endpoints (URL paths)
    echo ""
    echo "  Top 10 Most Requested Endpoints:"
    echo "  ─────────────────────────────────"
    tail -n "$lines" "$log_file" \
        | awk '{print $7}' \
        | cut -d'?' -f1 \
        | sort | uniq -c | sort -rn \
        | head -10 \
        | while read -r count path; do
            printf "  %8d  %s\n" "$count" "$path"
        done

    # Top 5xx errors
    echo ""
    echo "  Top 5xx Errors:"
    echo "  ─────────────────────────────────"
    tail -n "$lines" "$log_file" \
        | awk '$9 ~ /^5/ {print $9, $7}' \
        | sort | uniq -c | sort -rn \
        | head -10 \
        | while read -r count code path; do
            printf "  %8d  %s  %s\n" "$count" "$code" "$path"
        done

    # Top IP addresses by request count
    echo ""
    echo "  Top 10 Client IPs:"
    echo "  ─────────────────────────────────"
    tail -n "$lines" "$log_file" \
        | awk '{print $1}' \
        | sort | uniq -c | sort -rn \
        | head -10 \
        | while read -r count ip; do
            printf "  %8d  %s\n" "$count" "$ip"
        done

    # Requests per hour (traffic distribution)
    echo ""
    echo "  Traffic by Hour:"
    echo "  ─────────────────────────────────"
    tail -n "$lines" "$log_file" \
        | awk '{print $4}' \
        | cut -d: -f2 \
        | sort | uniq -c \
        | while read -r count hour; do
            # Draw a simple bar chart
            local bar_len=$((count / 50))
            printf "  %02d:00  %s %d\n" "$hour" "$(printf '█%.0s' $(seq 1 $((bar_len > 1 ? bar_len : 1))))" "$count"
        done

    echo ""
    echo "═══════════════════════════════════════════════════════"
}

# ─── APPLICATION LOG ERROR EXTRACTOR ─────────────────────────────
extract_errors() {
    local log_file="$1"
    local since_minutes="${2:-60}"

    local cutoff
    cutoff=$(date -d "-${since_minutes} minutes" +'%Y-%m-%d %H:%M' 2>/dev/null || \
             date -v "-${since_minutes}M" +'%Y-%m-%d %H:%M')

    echo "Errors in last ${since_minutes} minutes (since ${cutoff}):"
    echo "─────────────────────────────────────────────────────────"

    local error_count=0
    while IFS= read -r line; do
        if [[ "$line" =~ ERROR|EXCEPTION|FATAL|CRITICAL ]]; then
            echo "$line"
            ((error_count++))
        fi
    done < "$log_file"

    echo "─────────────────────────────────────────────────────────"
    echo "Total errors: ${error_count}"

    return $((error_count > 0 ? 1 : 0))
}

# ─── MAIN ─────────────────────────────────────────────────────────
main() {
    local log_file="${1:-/var/log/nginx/access.log}"
    local mode="${2:-nginx}"

    if [[ ! -f "$log_file" ]]; then
        echo "Error: Log file not found: ${log_file}" >&2
        exit 1
    fi

    case "$mode" in
        nginx)   analyze_nginx "$log_file" ;;
        errors)  extract_errors "$log_file" ;;
        *)       echo "Unknown mode: $mode" >&2; exit 1 ;;
    esac
}

main "$@"
```

---

---

## Lesson 16 — Server Health Check Tool

> **Goal:** Build a comprehensive server health check that runs before deployments

---

### 16.1 Pre-Deployment Health Check Script

```bash
#!/usr/bin/env bash
# =============================================================================
# health_check.sh — Comprehensive pre/post deployment health check
#
# USAGE:
#   ./health_check.sh --env production
#   ./health_check.sh --env staging --format json
# =============================================================================
set -euo pipefail

readonly SCRIPT_NAME="$(basename "$0")"
GREEN='\033[0;32m'; RED='\033[0;31m'; YELLOW='\033[1;33m'; NC='\033[0m'

# Results storage
PASS=0; FAIL=0; WARN=0

check() {
    local name="$1"
    local status="$2"   # pass, fail, warn
    local message="$3"

    case "$status" in
        pass) echo -e "  ${GREEN}✅${NC} ${name}: ${message}"; ((PASS++)) ;;
        warn) echo -e "  ${YELLOW}⚠️ ${NC} ${name}: ${message}"; ((WARN++)) ;;
        fail) echo -e "  ${RED}❌${NC} ${name}: ${message}"; ((FAIL++)) ;;
    esac
}

# ─── KUBERNETES HEALTH ────────────────────────────────────────────
check_kubernetes() {
    local namespace="$1"

    echo ""
    echo "Kubernetes Cluster:"
    echo "─────────────────────────────────────────────────────"

    # Cluster connectivity
    if kubectl cluster-info &>/dev/null 2>&1; then
        check "cluster-connectivity" "pass" "API server reachable"
    else
        check "cluster-connectivity" "fail" "Cannot reach Kubernetes API server"
        return 1
    fi

    # Node readiness
    local total_nodes not_ready
    total_nodes=$(kubectl get nodes --no-headers 2>/dev/null | wc -l | tr -d ' ')
    not_ready=$(kubectl get nodes --no-headers 2>/dev/null | grep -v " Ready" | wc -l | tr -d ' ')

    if [[ $not_ready -eq 0 ]]; then
        check "node-readiness" "pass" "All ${total_nodes} nodes are Ready"
    else
        check "node-readiness" "fail" "${not_ready}/${total_nodes} nodes are NOT Ready"
    fi

    # Deployment health
    local failed_deployments
    failed_deployments=$(kubectl get deployments -n "$namespace" \
        --no-headers 2>/dev/null | \
        awk '$3 < $2 {print $1}' | wc -l | tr -d ' ')

    if [[ $failed_deployments -eq 0 ]]; then
        check "deployments" "pass" "All deployments in ${namespace} are healthy"
    else
        check "deployments" "fail" "${failed_deployments} deployments have insufficient replicas in ${namespace}"
    fi

    # Pod status
    local pending_pods crashloop_pods
    pending_pods=$(kubectl get pods -n "$namespace" --field-selector=status.phase=Pending \
        --no-headers 2>/dev/null | wc -l | tr -d ' ')
    crashloop_pods=$(kubectl get pods -n "$namespace" --no-headers 2>/dev/null | \
        grep -c "CrashLoopBackOff\|Error" || true)

    [[ $pending_pods -eq 0 ]] && \
        check "pending-pods" "pass" "No pending pods" || \
        check "pending-pods" "warn" "${pending_pods} pods are Pending"

    [[ $crashloop_pods -eq 0 ]] && \
        check "crashloop-pods" "pass" "No crash-looping pods" || \
        check "crashloop-pods" "fail" "${crashloop_pods} pods in CrashLoopBackOff or Error state"
}

# ─── SERVICE ENDPOINTS ────────────────────────────────────────────
check_endpoints() {
    local -A endpoints
    endpoints["api-health"]="http://api-service/health"
    endpoints["web-root"]="http://web-service/"

    echo ""
    echo "Service Endpoints:"
    echo "─────────────────────────────────────────────────────"

    for name in "${!endpoints[@]}"; do
        local url="${endpoints[$name]}"
        local http_code
        http_code=$(curl -sf -o /dev/null -w "%{http_code}" \
            --max-time 10 "${url}" 2>/dev/null || echo "000")

        if [[ "$http_code" == "200" ]]; then
            check "$name" "pass" "HTTP ${http_code}"
        elif [[ "$http_code" == "000" ]]; then
            check "$name" "fail" "Connection refused or timeout"
        else
            check "$name" "fail" "HTTP ${http_code}"
        fi
    done
}

# ─── CERTIFICATE EXPIRY ───────────────────────────────────────────
check_certificates() {
    local domains=("api.mycompany.com" "www.mycompany.com")

    echo ""
    echo "SSL Certificates:"
    echo "─────────────────────────────────────────────────────"

    for domain in "${domains[@]}"; do
        local expiry_date days_remaining

        expiry_date=$(echo | openssl s_client -servername "$domain" \
            -connect "${domain}:443" 2>/dev/null | \
            openssl x509 -noout -dates 2>/dev/null | \
            grep notAfter | cut -d= -f2 || echo "")

        if [[ -z "$expiry_date" ]]; then
            check "cert:${domain}" "warn" "Could not retrieve certificate"
            continue
        fi

        local expiry_epoch today_epoch
        expiry_epoch=$(date -d "$expiry_date" +%s 2>/dev/null || \
                       date -j -f "%b %d %H:%M:%S %Y %Z" "$expiry_date" +%s)
        today_epoch=$(date +%s)
        days_remaining=$(( (expiry_epoch - today_epoch) / 86400 ))

        if [[ $days_remaining -lt 7 ]]; then
            check "cert:${domain}" "fail" "Expires in ${days_remaining} days!"
        elif [[ $days_remaining -lt 30 ]]; then
            check "cert:${domain}" "warn" "Expires in ${days_remaining} days"
        else
            check "cert:${domain}" "pass" "Valid for ${days_remaining} more days"
        fi
    done
}

# ─── SUMMARY ─────────────────────────────────────────────────────
print_summary() {
    local total=$((PASS + FAIL + WARN))
    echo ""
    echo "═══════════════════════════════════════════════════════"
    echo "  Health Check Summary"
    echo "  ─────────────────────────────────────────────────────"
    printf "  ${GREEN}✅ Passed${NC}  : %d\n" "$PASS"
    printf "  ${YELLOW}⚠️  Warnings${NC}: %d\n" "$WARN"
    printf "  ${RED}❌ Failed${NC}  : %d\n" "$FAIL"
    echo "  ─────────────────────────────────────────────────────"
    if [[ $FAIL -gt 0 ]]; then
        printf "  ${RED}RESULT: UNHEALTHY ❌${NC}\n"
    elif [[ $WARN -gt 0 ]]; then
        printf "  ${YELLOW}RESULT: DEGRADED ⚠️${NC}\n"
    else
        printf "  ${GREEN}RESULT: HEALTHY ✅${NC}\n"
    fi
    echo "═══════════════════════════════════════════════════════"
}

# ─── MAIN ─────────────────────────────────────────────────────────
main() {
    local namespace="${1:-production}"

    echo "═══════════════════════════════════════════════════════"
    echo "  Infrastructure Health Check"
    echo "  Environment: ${namespace}"
    echo "  $(date +'%Y-%m-%d %H:%M:%S')"
    echo "═══════════════════════════════════════════════════════"

    check_kubernetes "$namespace" || true
    check_endpoints || true
    check_certificates || true
    print_summary

    [[ $FAIL -gt 0 ]] && exit 2
    [[ $WARN -gt 0 ]] && exit 1
    exit 0
}

main "$@"
```

---

---

## Lesson 17 — Deployment Automation Scripts

> **Goal:** Build complete deployment pipelines in Bash

---

### 17.1 Zero-Downtime Kubernetes Deployment Script

```bash
#!/usr/bin/env bash
# =============================================================================
# k8s_deploy.sh — Zero-downtime Kubernetes deployment with automatic rollback
# =============================================================================
set -euo pipefail

readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
GREEN='\033[0;32m'; RED='\033[0;31m'; YELLOW='\033[1;33m'; NC='\033[0m'

log()     { echo -e "${GREEN}[$(date +'%H:%M:%S')]${NC} $*"; }
warn()    { echo -e "${YELLOW}[$(date +'%H:%M:%S')] ⚠️ ${NC} $*" >&2; }
error()   { echo -e "${RED}[$(date +'%H:%M:%S')] ❌${NC} $*" >&2; }
success() { echo -e "${GREEN}[$(date +'%H:%M:%S')] ✅${NC} $*"; }

# Rollback on script failure
DEPLOYMENT_UPDATED=false
APP_NAME=""
NAMESPACE=""

rollback_on_failure() {
    local exit_code=$?
    if [[ $exit_code -ne 0 && "$DEPLOYMENT_UPDATED" == "true" ]]; then
        error "Deployment failed — initiating automatic rollback..."
        kubectl rollout undo "deployment/${APP_NAME}" -n "${NAMESPACE}" 2>/dev/null || true
        warn "Rollback initiated. Check: kubectl rollout status deployment/${APP_NAME} -n ${NAMESPACE}"
    fi
}
trap rollback_on_failure EXIT

# ─── DEPLOY FUNCTION ────────────────────────────────────────────
k8s_deploy() {
    APP_NAME="$1"
    local image_tag="$2"
    NAMESPACE="${3:-production}"
    local timeout="${4:-300}"
    local registry="${REGISTRY:-mycompany.azurecr.io}"

    log "Starting deployment:"
    log "  App       : ${APP_NAME}"
    log "  Image tag : ${image_tag}"
    log "  Namespace : ${NAMESPACE}"
    log "  Registry  : ${registry}"

    # ─── Step 1: Verify preconditions ────────────────────────────
    log "Step 1: Verifying preconditions..."

    # Namespace exists
    kubectl get namespace "${NAMESPACE}" &>/dev/null || {
        error "Namespace '${NAMESPACE}' does not exist"
        exit 1
    }

    # Deployment exists
    kubectl get deployment "${APP_NAME}" -n "${NAMESPACE}" &>/dev/null || {
        error "Deployment '${APP_NAME}' not found in namespace '${NAMESPACE}'"
        exit 1
    }

    # Image exists in registry
    local full_image="${registry}/${APP_NAME}:${image_tag}"
    if command -v docker &>/dev/null; then
        docker manifest inspect "${full_image}" &>/dev/null || {
            error "Image not found in registry: ${full_image}"
            exit 1
        }
    fi
    log "  Preconditions verified"

    # ─── Step 2: Record current state ────────────────────────────
    log "Step 2: Recording current state..."
    local current_image
    current_image=$(kubectl get deployment "${APP_NAME}" -n "${NAMESPACE}" \
        -o jsonpath="{.spec.template.spec.containers[0].image}")
    log "  Current image: ${current_image}"
    log "  New image    : ${full_image}"

    # ─── Step 3: Update image ────────────────────────────────────
    log "Step 3: Updating deployment image..."
    kubectl set image \
        "deployment/${APP_NAME}" \
        "${APP_NAME}=${full_image}" \
        -n "${NAMESPACE}"
    DEPLOYMENT_UPDATED=true
    log "  Image updated"

    # ─── Step 4: Annotate deployment ─────────────────────────────
    local deployer="${DEPLOYER:-$(whoami)}"
    local git_sha="${GIT_SHA:-unknown}"
    kubectl annotate deployment "${APP_NAME}" \
        -n "${NAMESPACE}" \
        --overwrite \
        "kubernetes.io/change-cause=Deploy ${full_image} by ${deployer}" \
        "deployment.mycompany.com/git-sha=${git_sha}" \
        "deployment.mycompany.com/deployed-at=$(date -u +%Y-%m-%dT%H:%M:%SZ)"

    # ─── Step 5: Wait for rollout ─────────────────────────────────
    log "Step 4: Waiting for rollout to complete (timeout: ${timeout}s)..."
    if kubectl rollout status \
        "deployment/${APP_NAME}" \
        -n "${NAMESPACE}" \
        --timeout="${timeout}s"; then
        success "Rollout complete!"
    else
        error "Rollout failed or timed out"
        exit 1
    fi

    # ─── Step 6: Verify pods are ready ────────────────────────────
    log "Step 5: Verifying pod health..."
    local ready_pods total_pods
    ready_pods=$(kubectl get deployment "${APP_NAME}" -n "${NAMESPACE}" \
        -o jsonpath="{.status.readyReplicas}" 2>/dev/null || echo 0)
    total_pods=$(kubectl get deployment "${APP_NAME}" -n "${NAMESPACE}" \
        -o jsonpath="{.spec.replicas}")

    if [[ "${ready_pods}" == "${total_pods}" ]]; then
        success "All ${ready_pods}/${total_pods} pods are ready"
    else
        error "Only ${ready_pods:-0}/${total_pods} pods are ready"
        exit 1
    fi

    # Trap won't trigger rollback on success
    DEPLOYMENT_UPDATED=false

    success "Deployment of ${APP_NAME}:${image_tag} to ${NAMESPACE} succeeded!"
}

# ─── MAIN ─────────────────────────────────────────────────────────
if [[ $# -lt 2 ]]; then
    echo "Usage: $0 <app-name> <image-tag> [namespace] [timeout]"
    echo "Example: $0 api v1.2.3 production 300"
    exit 2
fi

k8s_deploy "$@"
```

---

---

## Lesson 18 — Backup Automation and Cron Jobs

> **Goal:** Build reliable backup systems with scheduling and alerting

---

### 18.1 Database Backup Automation

```bash
#!/usr/bin/env bash
# =============================================================================
# db_backup.sh — Automated PostgreSQL backup with S3 upload and retention
#
# REQUIRED ENVIRONMENT VARIABLES:
#   DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD
#   S3_BUCKET, AWS_REGION
#   SLACK_WEBHOOK (optional)
# =============================================================================
set -euo pipefail

readonly SCRIPT_NAME="$(basename "$0")"
readonly TIMESTAMP="$(date +%Y%m%d_%H%M%S)"
readonly BACKUP_DIR="/tmp/db_backups"
readonly RETENTION_DAYS="${RETENTION_DAYS:-30}"

GREEN='\033[0;32m'; RED='\033[0;31m'; NC='\033[0m'
log()   { echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*"; }
error() { echo "[$(date +'%Y-%m-%d %H:%M:%S')] ERROR: $*" >&2; }

# Cleanup temp files on exit
BACKUP_FILE=""
cleanup() {
    [[ -n "$BACKUP_FILE" && -f "$BACKUP_FILE" ]] && rm -f "$BACKUP_FILE"
}
trap cleanup EXIT

notify() {
    local status="$1"
    local message="$2"
    [[ -z "${SLACK_WEBHOOK:-}" ]] && return 0

    local icon; [[ "$status" == "success" ]] && icon="✅" || icon="❌"
    curl -sf -X POST "${SLACK_WEBHOOK}" \
        -H 'Content-type: application/json' \
        -d "{\"text\": \"${icon} DB Backup ${status}: ${message}\"}" &>/dev/null || true
}

require_var() {
    local var_name="$1"
    if [[ -z "${!var_name:-}" ]]; then
        error "Required environment variable '${var_name}' is not set"
        exit 1
    fi
}

main() {
    # Validate required variables
    for var in DB_HOST DB_PORT DB_NAME DB_USER DB_PASSWORD S3_BUCKET; do
        require_var "$var"
    done

    mkdir -p "${BACKUP_DIR}"
    BACKUP_FILE="${BACKUP_DIR}/${DB_NAME}_${TIMESTAMP}.sql.gz"
    local s3_key="backups/${DB_NAME}/${DB_NAME}_${TIMESTAMP}.sql.gz"

    log "Starting backup: ${DB_NAME} → s3://${S3_BUCKET}/${s3_key}"

    # ─── Step 1: Create database dump ────────────────────────────
    log "Dumping database..."
    PGPASSWORD="${DB_PASSWORD}" pg_dump \
        --host="${DB_HOST}" \
        --port="${DB_PORT}" \
        --username="${DB_USER}" \
        --dbname="${DB_NAME}" \
        --format=plain \
        --no-password \
        --verbose \
        2>/dev/null | gzip -9 > "${BACKUP_FILE}"

    local backup_size
    backup_size=$(du -sh "${BACKUP_FILE}" | cut -f1)
    log "Dump complete: ${BACKUP_FILE} (${backup_size})"

    # ─── Step 2: Verify backup is not empty ──────────────────────
    local min_size_bytes=1024   # At least 1KB
    local actual_size_bytes
    actual_size_bytes=$(stat -c%s "${BACKUP_FILE}" 2>/dev/null || stat -f%z "${BACKUP_FILE}")

    if [[ $actual_size_bytes -lt $min_size_bytes ]]; then
        error "Backup file is suspiciously small (${actual_size_bytes} bytes) — aborting"
        notify "failed" "Backup too small: ${actual_size_bytes} bytes"
        exit 1
    fi

    # ─── Step 3: Upload to S3 ────────────────────────────────────
    log "Uploading to s3://${S3_BUCKET}/${s3_key}..."
    aws s3 cp "${BACKUP_FILE}" "s3://${S3_BUCKET}/${s3_key}" \
        --storage-class STANDARD_IA \
        --region "${AWS_REGION:-us-east-1}"

    log "Upload complete"

    # ─── Step 4: Apply retention policy ──────────────────────────
    log "Applying retention policy (keep last ${RETENTION_DAYS} days)..."
    local cutoff_date
    cutoff_date=$(date -d "-${RETENTION_DAYS} days" +%Y-%m-%d 2>/dev/null || \
                  date -v "-${RETENTION_DAYS}d" +%Y-%m-%d)

    aws s3 ls "s3://${S3_BUCKET}/backups/${DB_NAME}/" \
        --region "${AWS_REGION:-us-east-1}" | \
    while read -r line; do
        local file_date file_name
        file_date=$(echo "$line" | awk '{print $1}')
        file_name=$(echo "$line" | awk '{print $4}')

        if [[ "$file_date" < "$cutoff_date" && -n "$file_name" ]]; then
            log "Removing old backup: ${file_name} (${file_date})"
            aws s3 rm "s3://${S3_BUCKET}/backups/${DB_NAME}/${file_name}" \
                --region "${AWS_REGION:-us-east-1}" || true
        fi
    done

    log "Backup completed successfully!"
    notify "success" "${DB_NAME} backup (${backup_size}) → s3://${S3_BUCKET}/${s3_key}"
}

main "$@"
```

---

### 18.2 Cron Job Setup

```bash
#!/usr/bin/env bash
# setup_cron.sh — Install automation cron jobs

set -euo pipefail

SCRIPTS_DIR="/opt/automation"
LOG_DIR="/var/log/automation"

mkdir -p "$LOG_DIR"

# Install scripts
chmod +x "${SCRIPTS_DIR}"/*.sh

# Add cron jobs (using crontab -l to preserve existing jobs)
# Format: minute hour day month weekday command
CRON_JOBS=(
    # Database backup — daily at 2:00am
    "0 2 * * * ${SCRIPTS_DIR}/db_backup.sh >> ${LOG_DIR}/db_backup.log 2>&1"

    # System monitor — every 5 minutes
    "*/5 * * * * ${SCRIPTS_DIR}/system_monitor.sh --once >> ${LOG_DIR}/monitor.log 2>&1"

    # Log archival — weekly on Sundays at 3:00am
    "0 3 * * 0 ${SCRIPTS_DIR}/log_archiver.py --log-dir /var/log/app --archive-dir /backup/logs >> ${LOG_DIR}/archiver.log 2>&1"

    # Health check — every 10 minutes
    "*/10 * * * * ${SCRIPTS_DIR}/health_check.sh production >> ${LOG_DIR}/health.log 2>&1"

    # Log rotation — daily at midnight
    "0 0 * * * /usr/sbin/logrotate /etc/logrotate.d/automation"
)

# Install cron jobs safely
(crontab -l 2>/dev/null || true) > /tmp/current_crontab

for job in "${CRON_JOBS[@]}"; do
    if ! grep -qF "$job" /tmp/current_crontab; then
        echo "$job" >> /tmp/current_crontab
        echo "Added: $job"
    else
        echo "Already exists: $(echo $job | awk '{print $1,$2,$3,$4,$5}')"
    fi
done

crontab /tmp/current_crontab
rm /tmp/current_crontab
echo "Cron jobs installed successfully"
crontab -l
```


---

---

# PHASE 4 — ADVANCED DEVOPS AUTOMATION

---

## Lesson 19 — Python AWS Automation with Boto3

> **Goal:** Automate AWS infrastructure using Python's official AWS SDK

---

### 19.1 Boto3 Setup and Authentication

```bash
pip install boto3
```

```python
import boto3
import os
from typing import Optional

# ─── AUTHENTICATION METHODS ──────────────────────────────────────
# Method 1: Environment variables (CI/CD)
# AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION

# Method 2: IAM Role (EC2/Lambda/ECS — no credentials in code!)
# This is the production standard — your instance/function assumes a role
ec2 = boto3.client("ec2")   # Automatically uses the instance role

# Method 3: Named profiles (~/.aws/credentials)
session = boto3.Session(profile_name="mycompany-production")
ec2 = session.client("ec2", region_name="ap-south-1")

# Method 4: Explicit credentials (only for testing)
ec2 = boto3.client(
    "ec2",
    region_name="ap-south-1",
    aws_access_key_id=os.environ["AWS_ACCESS_KEY_ID"],
    aws_secret_access_key=os.environ["AWS_SECRET_ACCESS_KEY"],
)

# ─── BOTO3 RESOURCE vs CLIENT ─────────────────────────────────────
# client  — low-level, 1:1 mapping to AWS API, returns dicts
# resource — high-level, object-oriented, more Pythonic

# Client example:
ec2_client = boto3.client("ec2", region_name="ap-south-1")
response = ec2_client.describe_instances()
# Returns a large nested dict — you have to navigate it

# Resource example:
ec2_resource = boto3.resource("ec2", region_name="ap-south-1")
for instance in ec2_resource.instances.all():
    print(instance.instance_id)   # Object attributes — easier!

# ─── ERROR HANDLING ───────────────────────────────────────────────
from botocore.exceptions import ClientError, BotoCoreError

def safe_aws_call(func, *args, **kwargs):
    """Wrap AWS API calls with standardised error handling."""
    try:
        return func(*args, **kwargs)
    except ClientError as e:
        error_code = e.response["Error"]["Code"]
        error_msg  = e.response["Error"]["Message"]
        raise RuntimeError(f"AWS API error [{error_code}]: {error_msg}") from e
    except BotoCoreError as e:
        raise RuntimeError(f"AWS SDK error: {e}") from e
```

---

### 19.2 EC2 Automation

```python
#!/usr/bin/env python3
"""
ec2_manager.py — EC2 instance lifecycle automation.

Handles: launch, stop, start, terminate, list, status.
"""
import boto3
import argparse
import json
import sys
import logging
import time
from typing import List, Optional, Dict

logger = logging.getLogger(__name__)


class EC2Manager:
    def __init__(self, region: str = "ap-south-1"):
        self.region = region
        self.ec2 = boto3.resource("ec2", region_name=region)
        self.client = boto3.client("ec2", region_name=region)

    def list_instances(self, filters: list = None, running_only: bool = False) -> List[dict]:
        """List EC2 instances with their details."""
        params = {}
        if filters:
            params["Filters"] = filters
        elif running_only:
            params["Filters"] = [{"Name": "instance-state-name", "Values": ["running"]}]

        instances = []
        paginator = self.client.get_paginator("describe_instances")

        for page in paginator.paginate(**params):
            for reservation in page["Reservations"]:
                for instance in reservation["Instances"]:
                    name = next(
                        (tag["Value"] for tag in instance.get("Tags", [])
                         if tag["Key"] == "Name"),
                        "unnamed"
                    )
                    instances.append({
                        "id":          instance["InstanceId"],
                        "name":        name,
                        "type":        instance["InstanceType"],
                        "state":       instance["State"]["Name"],
                        "private_ip":  instance.get("PrivateIpAddress", "-"),
                        "public_ip":   instance.get("PublicIpAddress", "-"),
                        "az":          instance["Placement"]["AvailabilityZone"],
                        "launch_time": instance["LaunchTime"].isoformat(),
                    })

        return instances

    def get_instance(self, instance_id: str) -> dict:
        """Get details of a specific instance."""
        instances = self.list_instances(
            filters=[{"Name": "instance-id", "Values": [instance_id]}]
        )
        if not instances:
            raise ValueError(f"Instance {instance_id} not found")
        return instances[0]

    def stop_instances(self, instance_ids: List[str], wait: bool = True) -> bool:
        """Stop one or more instances."""
        logger.info(f"Stopping instances: {', '.join(instance_ids)}")

        response = self.client.stop_instances(InstanceIds=instance_ids)
        stopping = [i["InstanceId"] for i in response["StoppingInstances"]]
        logger.info(f"  Stop initiated for: {', '.join(stopping)}")

        if wait:
            logger.info("  Waiting for instances to stop...")
            waiter = self.client.get_waiter("instance_stopped")
            waiter.wait(InstanceIds=instance_ids)
            logger.info("  All instances stopped")

        return True

    def start_instances(self, instance_ids: List[str], wait: bool = True) -> bool:
        """Start stopped instances."""
        logger.info(f"Starting instances: {', '.join(instance_ids)}")

        response = self.client.start_instances(InstanceIds=instance_ids)
        starting = [i["InstanceId"] for i in response["StartingInstances"]]
        logger.info(f"  Start initiated for: {', '.join(starting)}")

        if wait:
            logger.info("  Waiting for instances to be running...")
            waiter = self.client.get_waiter("instance_running")
            waiter.wait(InstanceIds=instance_ids)
            logger.info("  All instances running")

        return True

    def create_snapshot(self, volume_id: str, description: str, tags: dict = None) -> str:
        """Create an EBS snapshot and return the snapshot ID."""
        tag_specs = []
        if tags:
            tag_specs = [{
                "ResourceType": "snapshot",
                "Tags": [{"Key": k, "Value": v} for k, v in tags.items()]
            }]

        response = self.client.create_snapshot(
            VolumeId=volume_id,
            Description=description,
            TagSpecifications=tag_specs,
        )
        snapshot_id = response["SnapshotId"]
        logger.info(f"Created snapshot: {snapshot_id}")

        # Wait for completion
        waiter = self.client.get_waiter("snapshot_completed")
        waiter.wait(SnapshotIds=[snapshot_id])
        logger.info(f"Snapshot {snapshot_id} completed")

        return snapshot_id

    def cleanup_old_snapshots(self, days_old: int = 30, dry_run: bool = True) -> int:
        """Delete EBS snapshots older than N days. Returns count deleted."""
        from datetime import datetime, timezone, timedelta
        from botocore.exceptions import ClientError

        cutoff = datetime.now(timezone.utc) - timedelta(days=days_old)
        deleted = 0

        paginator = self.client.get_paginator("describe_snapshots")
        for page in paginator.paginate(OwnerIds=["self"]):
            for snapshot in page["Snapshots"]:
                if snapshot["StartTime"] < cutoff:
                    snap_id = snapshot["SnapshotId"]
                    age_days = (datetime.now(timezone.utc) - snapshot["StartTime"]).days

                    if dry_run:
                        logger.info(f"  [DRY RUN] Would delete: {snap_id} ({age_days}d old)")
                    else:
                        try:
                            self.client.delete_snapshot(SnapshotId=snap_id)
                            logger.info(f"  Deleted: {snap_id} ({age_days}d old)")
                            deleted += 1
                        except ClientError as e:
                            if "InvalidSnapshot.InUse" in str(e):
                                logger.warning(f"  Skipping in-use snapshot: {snap_id}")
                            else:
                                raise

        return deleted


def main() -> None:
    parser = argparse.ArgumentParser(description="EC2 instance manager")
    parser.add_argument("--region", default="ap-south-1")
    parser.add_argument("-v", "--verbose", action="store_true")

    subparsers = parser.add_subparsers(dest="command", required=True)

    list_p = subparsers.add_parser("list", help="List instances")
    list_p.add_argument("--running-only", action="store_true")
    list_p.add_argument("--format", choices=["table", "json"], default="table")

    stop_p = subparsers.add_parser("stop", help="Stop instances")
    stop_p.add_argument("--ids", nargs="+", required=True)
    stop_p.add_argument("--no-wait", action="store_true")

    start_p = subparsers.add_parser("start", help="Start instances")
    start_p.add_argument("--ids", nargs="+", required=True)

    cleanup_p = subparsers.add_parser("cleanup-snapshots")
    cleanup_p.add_argument("--days", type=int, default=30)
    cleanup_p.add_argument("--dry-run", action="store_true", default=True)
    cleanup_p.add_argument("--execute", action="store_true",
                           help="Actually delete (overrides --dry-run)")

    args = parser.parse_args()

    logging.basicConfig(
        level=logging.DEBUG if args.verbose else logging.INFO,
        format="%(asctime)s [%(levelname)s] %(message)s",
    )

    manager = EC2Manager(region=args.region)

    if args.command == "list":
        instances = manager.list_instances(running_only=args.running_only)
        if args.format == "json":
            print(json.dumps(instances, indent=2))
        else:
            print(f"\n{'─' * 100}")
            print(f"  {'ID':<20} {'NAME':<25} {'TYPE':<15} {'STATE':<12} {'PRIVATE IP':<16} {'AZ'}")
            print(f"{'─' * 100}")
            for i in instances:
                print(f"  {i['id']:<20} {i['name']:<25} {i['type']:<15} {i['state']:<12} {i['private_ip']:<16} {i['az']}")
            print(f"{'─' * 100}")
            print(f"  Total: {len(instances)} instances\n")

    elif args.command == "stop":
        manager.stop_instances(args.ids, wait=not args.no_wait)

    elif args.command == "start":
        manager.start_instances(args.ids)

    elif args.command == "cleanup-snapshots":
        dry_run = not args.execute
        count = manager.cleanup_old_snapshots(args.days, dry_run=dry_run)
        print(f"{'[DRY RUN] Would delete' if dry_run else 'Deleted'}: {count} snapshots")

    sys.exit(0)


if __name__ == "__main__":
    main()
```

---

### 19.3 S3 Automation

```python
import boto3
import os
from pathlib import Path
from typing import List

class S3Manager:
    def __init__(self, region: str = "ap-south-1"):
        self.s3 = boto3.client("s3", region_name=region)
        self.resource = boto3.resource("s3", region_name=region)

    def upload_directory(
        self,
        local_dir: Path,
        bucket: str,
        prefix: str = "",
        extra_args: dict = None,
    ) -> List[str]:
        """Upload an entire directory to S3."""
        uploaded = []
        extra = extra_args or {}

        for local_file in local_dir.rglob("*"):
            if not local_file.is_file():
                continue

            relative_path = local_file.relative_to(local_dir)
            s3_key = f"{prefix}/{relative_path}" if prefix else str(relative_path)
            s3_key = s3_key.replace("\\", "/")   # Windows path fix

            self.s3.upload_file(
                str(local_file),
                bucket,
                s3_key,
                ExtraArgs=extra,
            )
            uploaded.append(s3_key)

        return uploaded

    def sync_to_cloudfront(
        self,
        bucket: str,
        distribution_id: str,
        paths: List[str] = None,
    ) -> str:
        """Create a CloudFront invalidation after S3 upload."""
        cf = boto3.client("cloudfront")
        invalidation_paths = paths or ["/*"]

        response = cf.create_invalidation(
            DistributionId=distribution_id,
            InvalidationBatch={
                "Paths": {
                    "Quantity": len(invalidation_paths),
                    "Items": invalidation_paths,
                },
                "CallerReference": str(os.urandom(8).hex()),
            },
        )
        return response["Invalidation"]["Id"]

    def set_lifecycle_policy(self, bucket: str, days_to_ia: int = 30, days_to_glacier: int = 90) -> None:
        """Apply intelligent tiering lifecycle policy to a bucket."""
        self.s3.put_bucket_lifecycle_configuration(
            Bucket=bucket,
            LifecycleConfiguration={
                "Rules": [{
                    "ID": "intelligent-tiering",
                    "Status": "Enabled",
                    "Filter": {"Prefix": ""},
                    "Transitions": [
                        {"Days": days_to_ia,      "StorageClass": "STANDARD_IA"},
                        {"Days": days_to_glacier, "StorageClass": "GLACIER"},
                    ],
                    "NoncurrentVersionTransitions": [
                        {"NoncurrentDays": 30, "StorageClass": "GLACIER"},
                    ],
                    "NoncurrentVersionExpiration": {"NoncurrentDays": 90},
                }],
            },
        )
```

---

---

## Lesson 20 — Kubernetes Automation Scripts

> **Goal:** Automate Kubernetes operations with Python and kubectl

---

### 20.1 Python Kubernetes Client

```bash
pip install kubernetes
```

```python
#!/usr/bin/env python3
"""
k8s_automation.py — Kubernetes cluster automation using the Python client.

Operations: pod management, deployment scaling, log collection,
resource cleanup, cluster state reporting.
"""
import argparse
import json
import logging
import sys
from datetime import datetime, timezone, timedelta
from typing import List, Optional, Dict

from kubernetes import client, config
from kubernetes.client.rest import ApiException

logger = logging.getLogger(__name__)


class KubernetesManager:
    def __init__(self, kubeconfig: str = None, in_cluster: bool = False):
        """
        Initialise Kubernetes client.

        Args:
            kubeconfig: Path to kubeconfig file (default: ~/.kube/config)
            in_cluster: Use in-cluster service account (for scripts running inside K8s)
        """
        if in_cluster:
            config.load_incluster_config()
            logger.info("Using in-cluster configuration")
        else:
            config.load_kube_config(config_file=kubeconfig)
            logger.info(f"Using kubeconfig: {kubeconfig or '~/.kube/config'}")

        self.core = client.CoreV1Api()
        self.apps = client.AppsV1Api()
        self.batch = client.BatchV1Api()
        self.networking = client.NetworkingV1Api()

    def get_pods(
        self,
        namespace: str = "default",
        label_selector: str = None,
        field_selector: str = None,
    ) -> List[dict]:
        """List pods with their status."""
        try:
            pods = self.core.list_namespaced_pod(
                namespace=namespace,
                label_selector=label_selector,
                field_selector=field_selector,
            )
        except ApiException as e:
            raise RuntimeError(f"Failed to list pods: {e.status} {e.reason}") from e

        result = []
        for pod in pods.items:
            containers = pod.spec.containers
            statuses = pod.status.container_statuses or []

            ready_count = sum(1 for s in statuses if s.ready)
            restarts = sum(s.restart_count for s in statuses)

            result.append({
                "name": pod.metadata.name,
                "namespace": pod.metadata.namespace,
                "phase": pod.status.phase,
                "ready": f"{ready_count}/{len(containers)}",
                "restarts": restarts,
                "node": pod.spec.node_name,
                "ip": pod.status.pod_ip,
                "age_minutes": int(
                    (datetime.now(timezone.utc) - pod.metadata.creation_timestamp).total_seconds() / 60
                ),
            })

        return result

    def scale_deployment(
        self,
        name: str,
        namespace: str,
        replicas: int,
        wait: bool = True,
        timeout: int = 300,
    ) -> bool:
        """Scale a deployment to the specified replica count."""
        logger.info(f"Scaling {name} in {namespace} to {replicas} replicas")

        try:
            self.apps.patch_namespaced_deployment_scale(
                name=name,
                namespace=namespace,
                body={"spec": {"replicas": replicas}},
            )
        except ApiException as e:
            raise RuntimeError(f"Scale failed: {e.status} {e.reason}") from e

        if not wait:
            return True

        # Wait for rollout
        import time
        start = time.time()
        while time.time() - start < timeout:
            try:
                deployment = self.apps.read_namespaced_deployment(name, namespace)
                ready = deployment.status.ready_replicas or 0
                desired = replicas

                logger.debug(f"  Ready: {ready}/{desired}")

                if ready == desired:
                    logger.info(f"  Scale complete: {ready}/{desired} replicas ready")
                    return True
            except ApiException:
                pass

            time.sleep(5)

        raise TimeoutError(f"Deployment {name} did not scale within {timeout}s")

    def get_pod_logs(
        self,
        name: str,
        namespace: str,
        container: str = None,
        tail_lines: int = 100,
        previous: bool = False,
    ) -> str:
        """Get logs from a pod."""
        try:
            return self.core.read_namespaced_pod_log(
                name=name,
                namespace=namespace,
                container=container,
                tail_lines=tail_lines,
                previous=previous,
            )
        except ApiException as e:
            raise RuntimeError(f"Failed to get logs: {e.status} {e.reason}") from e

    def cleanup_completed_jobs(
        self,
        namespace: str = "default",
        older_than_hours: int = 24,
        dry_run: bool = True,
    ) -> int:
        """Delete completed/failed Jobs older than N hours."""
        cutoff = datetime.now(timezone.utc) - timedelta(hours=older_than_hours)
        deleted = 0

        try:
            jobs = self.batch.list_namespaced_job(namespace)
        except ApiException as e:
            raise RuntimeError(f"Failed to list jobs: {e}") from e

        for job in jobs.items:
            conditions = job.status.conditions or []
            is_complete = any(
                c.type in ("Complete", "Failed") for c in conditions
            )

            if not is_complete:
                continue

            completion_time = job.status.completion_time
            if completion_time and completion_time < cutoff:
                age_hours = int(
                    (datetime.now(timezone.utc) - completion_time).total_seconds() / 3600
                )

                if dry_run:
                    logger.info(f"  [DRY RUN] Would delete job: {job.metadata.name} ({age_hours}h old)")
                else:
                    try:
                        self.batch.delete_namespaced_job(
                            job.metadata.name,
                            namespace,
                            body=client.V1DeleteOptions(propagation_policy="Foreground"),
                        )
                        logger.info(f"  Deleted job: {job.metadata.name} ({age_hours}h old)")
                        deleted += 1
                    except ApiException as e:
                        logger.warning(f"  Failed to delete {job.metadata.name}: {e.status}")

        return deleted

    def cluster_summary(self) -> dict:
        """Get a summary of cluster resource usage."""
        summary = {
            "nodes": [],
            "namespaces": {},
            "total_pods": 0,
        }

        # Node information
        nodes = self.core.list_node()
        for node in nodes.items:
            conditions = {c.type: c.status for c in node.status.conditions}
            summary["nodes"].append({
                "name": node.metadata.name,
                "ready": conditions.get("Ready") == "True",
                "cpu": node.status.capacity.get("cpu"),
                "memory": node.status.capacity.get("memory"),
            })

        # Pod counts per namespace
        all_pods = self.core.list_pod_for_all_namespaces()
        for pod in all_pods.items:
            ns = pod.metadata.namespace
            if ns not in summary["namespaces"]:
                summary["namespaces"][ns] = {"pods": 0, "running": 0}
            summary["namespaces"][ns]["pods"] += 1
            if pod.status.phase == "Running":
                summary["namespaces"][ns]["running"] += 1
            summary["total_pods"] += 1

        return summary
```

---

---

## Lesson 21 — CI/CD Automation Scripting

> **Goal:** Write scripts that automate CI/CD pipeline operations

---

### 21.1 Complete CI/CD Automation Script

```bash
#!/usr/bin/env bash
# =============================================================================
# cicd_pipeline.sh — Complete CI/CD pipeline automation
#
# Stages: validate → build → test → package → deploy → verify
# Supports: GitHub Actions, GitLab CI, Jenkins, local execution
# =============================================================================
set -euo pipefail

readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly BUILD_ID="${BUILD_ID:-local-$(date +%Y%m%d%H%M%S)}"
readonly GIT_SHA="${GIT_COMMIT:-$(git rev-parse --short HEAD 2>/dev/null || echo 'unknown')}"
readonly GIT_BRANCH="${GIT_BRANCH:-$(git branch --show-current 2>/dev/null || echo 'unknown')}"

# Pipeline configuration
REGISTRY="${REGISTRY:-mycompany.azurecr.io}"
NAMESPACE="${NAMESPACE:-staging}"
TIMEOUT="${DEPLOY_TIMEOUT:-300}"

GREEN='\033[0;32m'; RED='\033[0;31m'; YELLOW='\033[1;33m'; BLUE='\033[0;34m'; NC='\033[0m'

# ─── LOGGING ─────────────────────────────────────────────────────
log()     { echo -e "${GREEN}[$(date +'%H:%M:%S')] ✓${NC} $*"; }
error()   { echo -e "${RED}[$(date +'%H:%M:%S')] ✗${NC} $*" >&2; }
stage()   { echo -e "\n${BLUE}╔════════════════════════════════════════╗${NC}"; \
            echo -e "${BLUE}║${NC} Stage: $* ${BLUE}║${NC}"; \
            echo -e "${BLUE}╚════════════════════════════════════════╝${NC}"; }
warn()    { echo -e "${YELLOW}[$(date +'%H:%M:%S')] ⚠${NC} $*"; }

# Pipeline timing
PIPELINE_START=$(date +%s)
declare -A STAGE_TIMES

time_stage() {
    local stage_name="$1"
    local stage_start=$(date +%s)
    "$@"
    local stage_end=$(date +%s)
    STAGE_TIMES["$stage_name"]=$((stage_end - stage_start))
}

# ─── STAGE FUNCTIONS ──────────────────────────────────────────────

stage_validate() {
    stage "VALIDATE"

    log "Git SHA     : ${GIT_SHA}"
    log "Git Branch  : ${GIT_BRANCH}"
    log "Build ID    : ${BUILD_ID}"

    # Check required tools
    local tools=("docker" "kubectl" "helm" "jq")
    for tool in "${tools[@]}"; do
        if command -v "$tool" &>/dev/null; then
            log "${tool} found: $(${tool} version --short 2>/dev/null || ${tool} --version 2>/dev/null | head -1)"
        else
            warn "${tool} not found — some stages may fail"
        fi
    done

    # Validate branch policy
    if [[ "$GIT_BRANCH" == "main" || "$GIT_BRANCH" == "master" ]]; then
        log "Deploying from default branch"
    elif [[ "$GIT_BRANCH" =~ ^release/ ]]; then
        log "Deploying release branch: ${GIT_BRANCH}"
    elif [[ "${CI:-false}" == "false" ]]; then
        warn "Local deployment from feature branch: ${GIT_BRANCH}"
    fi
}

stage_build() {
    stage "BUILD"

    local app_name="${1}"
    local dockerfile="${2:-Dockerfile}"
    local context="${3:-.}"

    local image="${REGISTRY}/${app_name}:${GIT_SHA}"
    log "Building: ${image}"

    docker build \
        --file "${dockerfile}" \
        --tag "${image}" \
        --label "git.sha=${GIT_SHA}" \
        --label "git.branch=${GIT_BRANCH}" \
        --label "build.id=${BUILD_ID}" \
        --label "build.timestamp=$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
        --cache-from "${REGISTRY}/${app_name}:latest" \
        --build-arg "BUILDKIT_INLINE_CACHE=1" \
        "${context}"

    log "Build complete: ${image}"

    # Also tag as latest on default branch
    if [[ "$GIT_BRANCH" == "main" || "$GIT_BRANCH" == "master" ]]; then
        docker tag "${image}" "${REGISTRY}/${app_name}:latest"
        log "Tagged as: ${REGISTRY}/${app_name}:latest"
    fi

    echo "${image}"   # Return the image name
}

stage_test() {
    stage "TEST"

    local app_name="$1"
    local image="${REGISTRY}/${app_name}:${GIT_SHA}"

    log "Running tests in container..."

    docker run --rm \
        --name "test-${app_name}-${BUILD_ID}" \
        -e CI=true \
        -e NODE_ENV=test \
        "${image}" \
        sh -c "npm test -- --ci --coverage --reporters=jest-junit" \
        2>&1 | tee test-output.log

    log "Tests passed"

    # Run security scan
    if command -v trivy &>/dev/null; then
        log "Running security scan..."
        trivy image \
            --exit-code 1 \
            --severity CRITICAL \
            --no-progress \
            "${image}" 2>/dev/null && log "Security scan passed" || {
            error "Security scan found CRITICAL vulnerabilities"
            return 1
        }
    else
        warn "trivy not available — skipping security scan"
    fi
}

stage_push() {
    stage "PUSH"

    local app_name="$1"
    local image="${REGISTRY}/${app_name}:${GIT_SHA}"

    log "Logging into registry: ${REGISTRY}"
    az acr login --name "$(echo $REGISTRY | cut -d. -f1)" 2>/dev/null || \
        docker login "${REGISTRY}" 2>/dev/null || \
        warn "Could not authenticate to registry — using existing session"

    log "Pushing: ${image}"
    docker push "${image}"

    if [[ "$GIT_BRANCH" == "main" || "$GIT_BRANCH" == "master" ]]; then
        docker push "${REGISTRY}/${app_name}:latest"
    fi

    log "Push complete"
}

stage_deploy() {
    stage "DEPLOY"

    local app_name="$1"
    local image="${REGISTRY}/${app_name}:${GIT_SHA}"

    log "Deploying ${image} to ${NAMESPACE}..."

    # Update Helm values
    helm upgrade --install "${app_name}" "${SCRIPT_DIR}/helm/${app_name}" \
        --namespace "${NAMESPACE}" \
        --create-namespace \
        --set "image.repository=${REGISTRY}/${app_name}" \
        --set "image.tag=${GIT_SHA}" \
        --set "environment=${NAMESPACE}" \
        --atomic \
        --wait \
        --timeout "${TIMEOUT}s" \
        --history-max 10

    log "Deployment triggered"

    # Wait for rollout
    log "Waiting for rollout..."
    if kubectl rollout status "deployment/${app_name}" \
        -n "${NAMESPACE}" \
        --timeout="${TIMEOUT}s"; then
        log "Rollout complete!"
    else
        error "Rollout failed!"
        log "Rolling back..."
        helm rollback "${app_name}" -n "${NAMESPACE}"
        return 1
    fi
}

stage_verify() {
    stage "VERIFY"

    local app_name="$1"
    local health_url="${2:-}"

    # Check pod status
    local ready_pods total_pods
    ready_pods=$(kubectl get deployment "${app_name}" -n "${NAMESPACE}" \
        -o jsonpath="{.status.readyReplicas}" 2>/dev/null || echo 0)
    total_pods=$(kubectl get deployment "${app_name}" -n "${NAMESPACE}" \
        -o jsonpath="{.spec.replicas}" 2>/dev/null || echo 0)

    if [[ "${ready_pods}" == "${total_pods}" && "$total_pods" -gt 0 ]]; then
        log "All ${ready_pods}/${total_pods} pods ready"
    else
        error "Only ${ready_pods:-0}/${total_pods} pods ready"
        return 1
    fi

    # HTTP health check
    if [[ -n "$health_url" ]]; then
        local max_tries=10
        for i in $(seq 1 $max_tries); do
            local status
            status=$(curl -sf -o /dev/null -w "%{http_code}" \
                --max-time 5 "${health_url}" 2>/dev/null || echo "000")

            if [[ "$status" == "200" ]]; then
                log "Health check passed (HTTP ${status})"
                break
            fi

            if [[ $i -eq $max_tries ]]; then
                error "Health check failed after ${max_tries} attempts (last: HTTP ${status})"
                return 1
            fi

            warn "Health check returned HTTP ${status} (attempt ${i}/${max_tries}) — retrying..."
            sleep 10
        done
    fi
}

# ─── PIPELINE SUMMARY ─────────────────────────────────────────────
print_summary() {
    local exit_code="$1"
    local pipeline_end=$(date +%s)
    local total_time=$((pipeline_end - PIPELINE_START))

    echo ""
    echo "╔═══════════════════════════════════════════════════════════╗"
    printf "║  Pipeline %-51s║\n" "$([[ $exit_code -eq 0 ]] && echo 'SUCCEEDED ✅' || echo 'FAILED ❌')"
    echo "║  ─────────────────────────────────────────────────────── ║"
    printf "║  Total time: %-48s║\n" "${total_time}s"
    printf "║  Build ID  : %-48s║\n" "${BUILD_ID}"
    printf "║  Git SHA   : %-48s║\n" "${GIT_SHA}"
    echo "║  ─────────────────────────────────────────────────────── ║"
    for stage_name in "${!STAGE_TIMES[@]}"; do
        printf "║    %-20s %-35s║\n" "$stage_name" "${STAGE_TIMES[$stage_name]}s"
    done
    echo "╚═══════════════════════════════════════════════════════════╝"
}

# ─── MAIN ─────────────────────────────────────────────────────────
main() {
    local app_name="${1:?Usage: $0 <app-name>}"

    trap 'print_summary $?; exit $?' EXIT

    time_stage stage_validate
    time_stage stage_build "$app_name"
    time_stage stage_test  "$app_name"
    time_stage stage_push  "$app_name"
    time_stage stage_deploy "$app_name"
    time_stage stage_verify "$app_name" "http://${app_name}.${NAMESPACE}.svc.cluster.local/health"
}

main "$@"
```

---

---

## Lesson 22 — Configuration Automation

> **Goal:** Manage configuration across environments, services, and infrastructure

---

### 22.1 Secrets Rotation Script

```python
#!/usr/bin/env python3
"""
rotate_secrets.py — Automated secrets rotation across services.

Rotates: database passwords, API keys, JWT secrets.
Updates: AWS Secrets Manager, Kubernetes secrets, application config.
"""
import argparse
import boto3
import base64
import json
import logging
import os
import secrets
import string
import sys
from datetime import datetime
from typing import Optional

logger = logging.getLogger(__name__)

def generate_password(length: int = 32, special_chars: bool = True) -> str:
    """Generate a cryptographically secure random password."""
    chars = string.ascii_letters + string.digits
    if special_chars:
        chars += "!#$%&*()-_=+[]{}|"

    while True:
        password = "".join(secrets.choice(chars) for _ in range(length))
        # Ensure it meets common complexity requirements
        has_upper   = any(c.isupper() for c in password)
        has_lower   = any(c.islower() for c in password)
        has_digit   = any(c.isdigit() for c in password)
        has_special = any(c in "!#$%&*()-_=+[]{}|" for c in password) or not special_chars

        if all([has_upper, has_lower, has_digit, has_special]):
            return password

def update_aws_secret(secret_name: str, new_value: str, region: str) -> str:
    """Update a secret in AWS Secrets Manager. Returns the version ID."""
    client = boto3.client("secretsmanager", region_name=region)

    response = client.update_secret(
        SecretId=secret_name,
        SecretString=new_value,
        Description=f"Rotated at {datetime.utcnow().isoformat()}Z",
    )
    version_id = response["VersionId"]
    logger.info(f"AWS Secrets Manager updated: {secret_name} (version: {version_id})")
    return version_id

def update_kubernetes_secret(
    secret_name: str,
    namespace: str,
    key: str,
    new_value: str,
) -> None:
    """Update a specific key in a Kubernetes Secret."""
    from kubernetes import client as k8s_client, config
    config.load_kube_config()

    core = k8s_client.CoreV1Api()

    # Encode value
    encoded = base64.b64encode(new_value.encode()).decode()

    try:
        # Try to patch existing secret
        core.patch_namespaced_secret(
            name=secret_name,
            namespace=namespace,
            body={"data": {key: encoded}},
        )
        logger.info(f"Kubernetes secret updated: {namespace}/{secret_name}[{key}]")
    except Exception as e:
        logger.error(f"Failed to update Kubernetes secret: {e}")
        raise


class SecretRotator:
    """Orchestrates secret rotation across multiple systems."""

    def __init__(self, region: str = "ap-south-1", dry_run: bool = True):
        self.region = region
        self.dry_run = dry_run

    def rotate_database_password(
        self,
        db_identifier: str,
        secret_name: str,
        k8s_namespace: str,
    ) -> bool:
        """Rotate database password across all dependent systems."""
        logger.info(f"Rotating database password for: {db_identifier}")

        new_password = generate_password(32, special_chars=False)  # RDS doesn't allow all specials
        logger.info(f"Generated new password (length: {len(new_password)})")

        if self.dry_run:
            logger.info(f"[DRY RUN] Would update:")
            logger.info(f"  AWS Secrets Manager: {secret_name}")
            logger.info(f"  Kubernetes secret: {k8s_namespace}/db-secret")
            return True

        # Step 1: Update AWS Secrets Manager
        version = update_aws_secret(
            secret_name,
            json.dumps({
                "username": "dbadmin",
                "password": new_password,
                "rotated_at": datetime.utcnow().isoformat() + "Z",
            }),
            self.region,
        )

        # Step 2: Update Kubernetes secret
        update_kubernetes_secret(
            "db-secret",
            k8s_namespace,
            "DB_PASSWORD",
            new_password,
        )

        # Step 3: Trigger rolling restart of dependent deployments
        from kubernetes import client as k8s_client, config
        config.load_kube_config()
        apps = k8s_client.AppsV1Api()

        for deployment_name in ["api", "worker"]:
            try:
                apps.patch_namespaced_deployment(
                    name=deployment_name,
                    namespace=k8s_namespace,
                    body={
                        "spec": {
                            "template": {
                                "metadata": {
                                    "annotations": {
                                        "secret.rotation/timestamp": datetime.utcnow().isoformat()
                                    }
                                }
                            }
                        }
                    },
                )
                logger.info(f"  Triggered rolling restart: {deployment_name}")
            except Exception as e:
                logger.warning(f"  Could not restart {deployment_name}: {e}")

        logger.info(f"Password rotation complete for: {db_identifier}")
        return True
```

---

---

## Lesson 23 — Complete DevOps Automation Toolkit

> **Goal:** A production-ready toolkit combining all learned automation patterns

---

### 23.1 The Complete Toolkit Script

```bash
#!/usr/bin/env bash
# =============================================================================
# devops_toolkit.sh — Unified DevOps automation command centre
#
# USAGE:
#   ./devops_toolkit.sh <command> [options]
#
# COMMANDS:
#   health   <env>              Run infrastructure health check
#   deploy   <app> <tag> <env>  Deploy application
#   rollback <app> <env>        Rollback last deployment
#   logs     <app> <env>        Tail application logs
#   scale    <app> <env> <n>    Scale deployment
#   backup   <db>               Trigger database backup
#   status   <env>              Show deployment status
# =============================================================================
set -euo pipefail

readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

GREEN='\033[0;32m'; RED='\033[0;31m'; YELLOW='\033[1;33m'; BLUE='\033[0;34m'; NC='\033[0m'

log()     { echo -e "${GREEN}[$(date +'%H:%M:%S')]${NC} $*"; }
error()   { echo -e "${RED}ERROR:${NC} $*" >&2; }
header()  { echo -e "\n${BLUE}▶${NC} $*"; }

# ─── COMMANDS ────────────────────────────────────────────────────
cmd_health() {
    local env="${1:-production}"
    header "Running health check for ${env}"
    python3 "${SCRIPT_DIR}/infra_check.py" --config "${SCRIPT_DIR}/configs/${env}.yml"
}

cmd_deploy() {
    local app="$1"
    local tag="$2"
    local env="${3:-staging}"
    header "Deploying ${app}:${tag} to ${env}"
    python3 "${SCRIPT_DIR}/deploy_orchestrator.py" \
        --app "$app" --tag "$tag" --env "$env"
}

cmd_rollback() {
    local app="$1"
    local env="${2:-production}"
    header "Rolling back ${app} in ${env}"
    helm rollback "$app" -n "$env" --wait
    log "Rollback complete"
}

cmd_logs() {
    local app="$1"
    local env="${2:-production}"
    header "Streaming logs for ${app} in ${env}"
    kubectl logs -n "$env" -l "app=${app}" -f --tail=100
}

cmd_scale() {
    local app="$1"
    local env="${2:-production}"
    local replicas="$3"
    header "Scaling ${app} in ${env} to ${replicas} replicas"
    kubectl scale deployment "$app" -n "$env" --replicas="$replicas"
    kubectl rollout status deployment/"$app" -n "$env"
    log "Scale complete"
}

cmd_status() {
    local env="${1:-production}"
    header "Deployment status — ${env}"
    echo ""
    printf "  %-30s %-20s %-10s\n" "DEPLOYMENT" "IMAGE" "READY"
    printf "  %-30s %-20s %-10s\n" "──────────────────────────────" "────────────────────" "──────────"

    kubectl get deployments -n "$env" \
        -o custom-columns=\
"NAME:.metadata.name,\
IMAGE:.spec.template.spec.containers[0].image,\
READY:.status.readyReplicas,\
DESIRED:.spec.replicas" \
        --no-headers | \
    while read -r name image ready desired; do
        local status_icon
        [[ "${ready:-0}" == "$desired" ]] && status_icon="${GREEN}✅${NC}" || status_icon="${RED}❌${NC}"
        printf "  %-30s %-20s " "$name" "$(echo $image | rev | cut -d/ -f1 | rev)"
        echo -e "${status_icon} ${ready:-0}/${desired}"
    done
    echo ""
}

# ─── HELP ────────────────────────────────────────────────────────
show_help() {
    grep '^#' "$0" | grep -v '#!/' | sed 's/^# \?//'
}

# ─── MAIN ─────────────────────────────────────────────────────────
main() {
    if [[ $# -eq 0 ]]; then
        show_help
        exit 0
    fi

    local command="$1"
    shift

    case "$command" in
        health)   cmd_health "$@" ;;
        deploy)   cmd_deploy "$@" ;;
        rollback) cmd_rollback "$@" ;;
        logs)     cmd_logs "$@" ;;
        scale)    cmd_scale "$@" ;;
        status)   cmd_status "$@" ;;
        help|-h|--help) show_help; exit 0 ;;
        *)
            error "Unknown command: ${command}"
            show_help
            exit 1
            ;;
    esac
}

main "$@"
```

---

### 23.2 DevOps Automation Best Practices Checklist

```
PYTHON SCRIPTS:
□ Use argparse for all CLI argument handling
□ Always sys.exit(0) success / sys.exit(1) failure
□ Use logging module, never print() for operational messages
□ Validate all inputs and environment variables at startup
□ Never hardcode credentials — use env vars or secrets managers
□ Wrap AWS/K8s API calls in try/except with meaningful messages
□ Use type hints on all function signatures
□ Add docstrings to all public functions
□ Test with --dry-run before production execution
□ Structure scripts with main() and if __name__ == "__main__"

BASH SCRIPTS:
□ Always start with: set -euo pipefail
□ Always quote variable expansions: "${VAR}"
□ Use [[ ]] not [ ] for conditionals
□ Declare local variables inside functions
□ Check for required tools with: command -v tool_name
□ Validate required env vars at the top of the script
□ Use trap for cleanup: trap cleanup EXIT
□ Log with timestamps: [$(date +'%Y-%m-%d %H:%M:%S')]
□ Return meaningful exit codes (0=success, 1=general, 2=usage)
□ Add --dry-run to all destructive scripts

BOTH:
□ Log start/end of every major operation
□ Include the script version in output (--version flag)
□ Document required environment variables
□ Add examples to usage/help text
□ Test on a non-production environment first
□ Peer review all scripts that touch production
□ Version control every script in Git
□ Use meaningful variable names (not $1, $2 — use $APP_NAME)
□ Handle errors gracefully — no silent failures
□ Send alerts/notifications on failure
```

---

## Official Documentation Reference

| Tool | URL |
|---|---|
| Python Documentation | https://docs.python.org/3/ |
| Bash Reference Manual | https://www.gnu.org/software/bash/manual/ |
| Linux Command Reference | https://linuxcommand.org/ |
| subprocess module | https://docs.python.org/3/library/subprocess.html |
| argparse module | https://docs.python.org/3/library/argparse.html |
| pathlib module | https://docs.python.org/3/library/pathlib.html |
| Boto3 (AWS SDK) | https://boto3.amazonaws.com/v1/documentation/api/latest/index.html |
| Kubernetes Python Client | https://github.com/kubernetes-client/python |
| PyYAML | https://pyyaml.org/wiki/PyYAMLDocumentation |
| requests library | https://requests.readthedocs.io/ |

---

## Learning Path

```
Week 1:   Lessons 1–2   Python CLI scripting, argparse, file automation
Week 2:   Lessons 3–4   Shell commands from Python, JSON/YAML processing
Week 3:   Lessons 5–6   Logging, API automation
Week 4:   Lessons 7–8   Deployment automation, infrastructure tool (Lab)
Week 5:   Lessons 9–10  Bash fundamentals, variables, arrays
Week 6:   Lessons 11–12 Bash conditionals, loops
Week 7:   Lesson 13     Bash functions, command substitution
Week 8:   Lessons 14–15 System monitoring, log analysis
Week 9:   Lessons 16–17 Health checks, deployment scripts
Week 10:  Lesson 18     Backup automation, cron jobs
Week 11:  Lesson 19     AWS automation with Boto3
Week 12:  Lesson 20     Kubernetes automation
Week 13:  Lesson 21     CI/CD automation scripting
Week 14:  Lessons 22–23 Configuration automation, complete toolkit
```

**Build Your Portfolio Automation Toolkit:**
Create a GitHub repository with:
- `health_check.py` — infrastructure health check
- `deploy.sh` — Kubernetes deployment script
- `monitor.sh` — system monitoring with Slack alerts
- `backup.sh` — database backup with S3 upload
- `log_analyzer.sh` — log analysis and error extraction
- `ec2_manager.py` — AWS EC2 automation
- `k8s_automation.py` — Kubernetes automation
- `rotate_secrets.py` — secrets rotation

All scripts should follow the patterns in this curriculum.

---

*Built on: Python Documentation · GNU Bash Manual · Linux Command Reference · AWS Boto3 Docs · Kubernetes Python Client*
