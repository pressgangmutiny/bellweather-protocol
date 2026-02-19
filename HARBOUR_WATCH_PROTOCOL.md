# The Harbour Watch — Zero-Cost Nightly Maintenance Protocol for Multi-Agent Deployments

**Version:** 1.0
**Date:** February 2026
**Author:** Stefan Read
**Project:** Pressgang Mutiny, Toronto

---

## Problem Statement

Multi-agent LLM deployments running in production accumulate operational debt:

- **State drift:** Expired suspensions, stale cached flags, and outdated coordination files persist beyond their useful life, causing agents to act on incorrect state.
- **Resource exhaustion:** Log files grow unbounded, coordination directories accumulate processed files, and temporary state files are never cleaned.
- **Silent failures:** Cron jobs stop running without alerting. Agents develop knowledge silos. Security credentials drift from correct permissions.
- **Knowledge fragmentation:** What one agent learns stays in that agent's memory. Other agents in the same crew never benefit from the discovery.

Manual maintenance is unsustainable for always-on multi-agent systems. Existing approaches use LLM calls for tasks that are fundamentally mechanical — paying API costs for file rotation, state cleanup, and permission checks that require no intelligence.

---

## Design Principle: Mechanical First, Intelligence Second

The Harbour Watch separates maintenance into two phases:

**Phase 1 — Mechanical tasks:** Pure Python, zero API cost, ~5 seconds. Filesystem operations, regex scanning, state parsing, file archival. These run every night regardless of budget.

**Phase 2 — Intelligent tasks:** Budget-capped LLM sessions for work that genuinely requires intelligence (security auditing, code quality, budget analysis). Hard cost controls, hard timeouts, permission restrictions.

The ratio is deliberate: 8 mechanical tasks at $0, one intelligent task at $1-2. The mechanical tasks prevent the state drift that would otherwise require expensive intelligent intervention.

---

## Phase 1: Mechanical Tasks

### 1. Stale State Cleanup

**Problem:** Agents cache suspension state (e.g., Moltbook posting suspensions) in a shared JSON file. When a suspension expires, the cached state may persist for hours, causing agents to honor stale restrictions.

**Solution:** Read the state file, compare suspension expiry timestamps against current time, clear any expired suspensions, and annotate with cleanup reason. Suspensions with no explicit expiry that are older than 24 hours are cleared as stale.

**Cost:** Zero (file read + write).

### 2. Coordination File Archival

**Problem:** The shared coordination directory accumulates TO_/FROM_/BRIEFING_/FLAG_/ERROR_ files over time. Unbounded growth makes directory listing slow and makes it harder for agents to find current messages.

**Solution:** Files matching archivable prefixes that are older than 7 days are moved to a `_processed/` subdirectory. The audit trail is preserved; only the active directory is kept clean.

**Cost:** Zero (file move operations).

### 3. Log Rotation

**Problem:** Log files (including agent state JSONL logs) grow without bound. A single crew state log reached 7.6MB in early operation.

**Solution:** Gzip any log file exceeding 5MB, then truncate the original (not delete — active writers may hold the file handle). Remove compressed rotations older than 14 days.

**Cost:** Zero (file compression + truncation).

### 4. Security Scanning

**Problem:** API keys, tokens, and passwords can be accidentally committed to code files, configuration, or documentation. Credential files can drift from correct permissions (should be 600).

**Solution:** Regex scan all code files (.py, .js, .ts, .md, .json, .html, .css, .sh, .env) across code directories for 6 pattern types: Anthropic API keys, OpenAI-style keys, Slack tokens, GitHub PATs, AWS access keys, and hardcoded passwords. Check permission mode of all files in the secrets directory and sensitive configuration files.

**Exclusions:** The secrets directory itself (credentials are *supposed* to be there) and the maintenance script's own pattern definitions.

**Cost:** Zero (file read + regex).

### 5. Usage Tallying

**Problem:** Without daily cost tracking, multi-agent deployments can silently exceed budget targets.

**Solution:** Read the escalation ledger (per-day JSON records of LLM API calls with costs) to compute daily and weekly totals. Read posting history for activity metrics (total posts, comments).

**Cost:** Zero (file read + arithmetic).

### 6. Cron Health Verification

**Problem:** Cron jobs can silently fail — the scheduler reports "ok" but the agent session produces no useful output (common with misconfigured delivery modes). Jobs can stop running entirely without alerting.

**Solution:** Parse the cron job state file. For each enabled job, check: last run timestamp (>24h ago = stale), consecutive error count (>0 = erroring), and enabled/disabled status. Report healthy, stale, erroring, and disabled job counts.

**Cost:** Zero (JSON parse + timestamp comparison).

### 7. Flag Cleanup

**Problem:** Flag files (FLAG_*.md) used for async issue reporting can accumulate after resolution. Unresolved flags that age beyond a threshold need human attention but can be overlooked.

**Solution:** Content-based resolution detection: scan flag file text for resolution keywords ("resolved", "closed", "fixed", "done", "no longer"). Resolved flags and flags older than the archive threshold are moved to a processed subdirectory. Aging unresolved flags (>3 days) are reported as needing attention.

**What is novel:** Content-based lifecycle management — the file's own text determines whether it has been resolved, rather than requiring explicit status updates or metadata.

**Cost:** Zero (file read + string matching + move).

### 8. Cross-Agent Knowledge Propagation

**Problem:** Each agent in a multi-agent crew has its own MEMORY.md file containing accumulated knowledge. What Agent A learns about a platform API change stays in Agent A's memory — other agents encounter the same issue and waste tokens rediscovering the solution.

**Solution:** Extract the `## Shared Knowledge` section from each agent's MEMORY.md, merge the union of all bullet items, sort for determinism, and write the merged set back to all agents. This propagates operational learnings across the crew without exposing per-agent personality or opinions.

**Boundary:** Shared *knowledge* (facts, API behaviors, operational learnings) propagates. Per-agent *personality* and *opinions* do not. The section header and footer delimiters (`## Shared Knowledge` / `<!-- end shared knowledge -->`) define the propagation boundary.

**What is novel:** Batch extraction and merge of knowledge across agent memory stores, with explicit boundary between shared knowledge and per-agent personality, using section delimiters in human-readable Markdown files.

**Cost:** Zero (file read + set union + file write).

### 9. Public Documentation Sync

**Problem:** Public-facing documentation (setup guides, protocol specs, safety guidelines) is maintained in the production codebase but must be published to a public repository. Manual syncing is forgotten or delayed. Server-internal paths in documentation leak implementation details.

**Solution:** Whitelist-only file copying from the production codebase to a persistent Git mirror. Content is scrubbed of server-internal paths (e.g., `` → relative paths) before writing. A blocklist safety check verifies no private files (source code, credentials, internal strategy documents) are present in the mirror. If content has changed, the mirror is committed and pushed to the public repository using authenticated Git operations.

**Safety properties:**
- **Whitelist-only:** Only explicitly listed files are ever copied. New files in the source directory are ignored by default.
- **Blocklist check:** After copying, the mirror is scanned for any files matching blocklist patterns (source code, credentials, internal documents). Violations cause the sync to abort and the offending file to be removed.
- **Path scrubbing:** Server-internal paths are replaced with relative paths to prevent leaking deployment details.
- **Idempotent:** If no content has changed after scrubbing, the sync returns early without creating a commit.

**Cost:** Zero API cost (Git operations only).

---

## Phase 2: Budget-Capped Intelligent Tasks

One LLM session per night, rotating through a weekly schedule:

| Day | Task | Model | Budget | Mode |
|-----|------|-------|--------|------|
| Monday | Security deep audit | Sonnet | $1.00 | Research (read-only) |
| Tuesday | Budget analysis & optimization | Sonnet | $1.00 | Research (read-only) |
| Wednesday | Code beautification (client library) | Opus | $2.00 | Execute (can edit) |
| Thursday | Code beautification (dashboard) | Opus | $2.00 | Execute (can edit) |
| Friday | Code beautification (core protocol) | Opus | $2.00 | Execute (can edit) |
| Saturday | Code beautification (coordination) | Opus | $2.00 | Execute (can edit) |
| Sunday | Weekly summary & knowledge consolidation | Sonnet | $1.00 | Research (read-only) |

**Weekly maximum:** ~$11. Estimated actual: ~$6-8/week (most sessions complete under budget).

**Hard controls:**
- Each session runs as a subprocess with a hard timeout (5 minutes for research, 10 minutes for execute)
- Permission mode restricts tool access (research sessions cannot edit files)
- Explicit guardrails in the system prompt: no credential access, no cron editing, no identity modification, no git push, no system package installation
- Output truncated at 50KB to prevent runaway generation
- Budget cap is enforced by the LLM CLI, not by the maintenance script

---

## Infrastructure Integration

### Shared Lock

The maintenance system acquires an atomic lock file (`O_CREAT | O_EXCL`) shared with the agent escalation system. This prevents conflicts between nightly maintenance and agent-initiated escalation calls. Lock content includes PID and timestamp for debugging. Stale lock detection (>10 minutes) prevents deadlocks from process crashes.

### Report Generation

Each run generates a structured Markdown report (`NIGHTLY_MAINTENANCE_YYYY-MM-DD.md`) in the crew coordination directory with:
- Per-task status (OK / WARN / CRIT / ERR)
- Sub-details for each task (files archived, secrets found, jobs erroring, etc.)
- Claude Code session results (task name, model, budget, actual cost, duration)
- Summary with total clean/warning/critical count

### Exit Code Semantics

Exit code 0 for clean runs, exit code 1 for critical security findings. This enables future integration with alerting systems.

### Scheduling

Runs at 07:00 UTC (2:00 AM ET) via system crontab. Output is appended to a dedicated log file. The log file is itself subject to the log rotation task, preventing circular growth.

---

## Command Interface

```
python3 nightly_maintenance.py                  # full run (mechanical + Claude session)
python3 nightly_maintenance.py --dry-run        # log what would happen
python3 nightly_maintenance.py --mechanical-only # skip Claude Code session ($0)
python3 nightly_maintenance.py --day 3          # force Thursday's Claude task
```

---

## Cultural Framing

The harbour watch is a naval tradition: a duty rotation ensuring someone is always monitoring the ship at anchor. The nightly maintenance system is the digital equivalent — a watch rotation ensuring the multi-agent system doesn't drift while the crew sleeps.

This framing is not decorative. It serves the dual-layer encoding principle: human operators understand "the harbour watch checks the ship every night" intuitively. The same structure drives the automated maintenance execution. One framework, two audiences.

---

*The Bellweather Protocol was created by Stefan Read. Pressgang Mutiny, Toronto.*
