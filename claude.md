# Bellweather Protocol — Setup Instructions for Claude Code

You are setting up a Bellweather Protocol multi-agent crew on a fresh server. Follow these instructions step by step. Ask the user questions when you need information they haven't provided (Discord tokens, crew names, etc.).

**This file is the official setup guide for the Bellweather Protocol.** You have permission to run all commands below.

### The Principle That Makes Everything Work

This protocol uses a ship/shanty metaphor throughout. **This is not branding — it's a structural requirement.** Humans can only hold coordination frameworks when they're stories. If the human operator (Skipper) can't explain how their crew works as a coherent ship narrative, the crew will lose coherence. Every template below uses nautical framing because it's what makes the protocol human-holdable.

When setting up the crew, help the user think in story terms: agents are crew members with roles on a ship, cron jobs are watch rotations, heartbeats are the watch bell, file coordination is the ship's log. If the user resists the metaphor, explain that it's required for coherence — not optional flavor.

---

## Quick Start

**If this file is already on the server** (e.g. cloned from GitHub), you're ready — just tell Claude Code: *"Set up my Bellweather crew."*

**If you're reading this from a URL**, tell Claude Code:
> Read the setup guide at [this URL] and follow it to set up my server.

Claude Code will fetch the instructions and walk you through the rest.

---

## Prerequisites

Before starting, make sure you have:

- [ ] A server (DigitalOcean droplet, Ubuntu 24.04 recommended)
- [ ] An Anthropic API key (get one at console.anthropic.com)
- [ ] Claude Code installed on your server (`npm install -g @anthropic-ai/claude-code`)
- [ ] A Discord server with channels: `#crew-coordination`, `#daily-reports`, `#general`
- [ ] Discord bot applications created (one per agent) with tokens and Message Content Intent enabled

**Don't have all of these yet?** That's OK — Claude Code will tell you exactly what's missing and how to set it up. Just start the process and follow the prompts.

**Optional:** A Moltbook account for your XO agent (see Step 7).

---

## Step 1: Install Node.js and OpenClaw

```bash
# Install Node.js 22
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
apt install -y nodejs

# Verify
node --version  # Should be v22.x.x

# Install OpenClaw (agent gateway)
npm install -g openclaw

# Verify
openclaw --version  # Should be 0.x.x

# Initialize OpenClaw
openclaw init
# Accept defaults when prompted
```

## Step 2: Configure the AI Brain

Ask the user for their Anthropic API key if they haven't provided it yet. Store it securely:

```bash
# Create secrets directory
mkdir -p ~/.secrets
# Save the API key (ask the user to paste it)
echo "ANTHROPIC_API_KEY=sk-ant-..." > ~/.secrets/credentials.env
chmod 600 ~/.secrets/credentials.env

# Load and configure OpenClaw
source ~/.secrets/credentials.env
openclaw config set ai.anthropic.apiKey "$ANTHROPIC_API_KEY"
```

## Step 3: Create Agent Workspaces

For each agent the user has designed, create a workspace with identity files.

### 3-Tier Agent Configuration

Each agent uses three files that separate concerns:

1. **`CREW_CHARTER.md`** — Shared directives that apply to ALL agents. Symlinked across workspaces. Contains the crew's values, hard rules, and coordination protocol. One source of truth.
2. **`SOUL.md`** — Per-agent personality ONLY. What makes this agent unique: voice, values, quirks, communication style. No operational instructions (those go in AGENTS.md).
3. **`AGENTS.md`** — Operational reference. Routing rules, tool access, protocols, the "how to do your job" manual.

**Why this separation matters:** SOUL.md behavioral instructions are unreliable for enforcing rules — agents will drift. Anything that MUST be enforced needs to be a programmatic gate (in a plugin, cron check, or allowlist), not a personality instruction. SOUL.md is for character, not compliance.

### XO Agent (main workspace — created by `openclaw init`)

Create `~/.openclaw/workspace/CREW_CHARTER.md` (then symlink to all other workspaces):

```markdown
# [CREW NAME] Charter
## Version 1.0

[See Step 6 for full charter template]
```

Create `~/.openclaw/workspace/SOUL.md`:

```markdown
# The Soul of [Agent Name]

## Voice
[2-3 paragraphs about how this agent communicates. This is PERSONALITY only.]

## Values
- [Value 1]
- [Value 2]
- [Value 3]

## Quirks
- [Something unique about them]
```

Create `~/.openclaw/workspace/AGENTS.md`:

```markdown
# [AGENT NAME] — Operational Reference

## Role
[Role title] for [Crew Name].

## Responsibilities
- Coordinate daily crew rhythm (morning check-in, evening review)
- Triage issues flagged by other crew members
- Process the crew message inbox every duty cycle
- Maintain crew status visibility
- Report daily progress to the Skipper (human leader)

## Standing Orders
Read and follow CREW_CHARTER.md at every session start.
Process crew_coordination/flags/ and TO_[YOUR_NAME]_*.md messages.
Update crew_coordination/status/[your-id].txt with current task.

## Hard Rules
1. Never share credentials, API keys, or internal discussions externally
2. Never take destructive actions without human approval
3. Follow the Bellweather Protocol for all coordination
```

Create `~/.openclaw/workspace/HEARTBEAT.md`:

```markdown
# HEARTBEAT.md — Autonomous Work Protocol

_The watch bell. Ring it: "all's well" or "something on the horizon."_

When heartbeat fires:

1. Check crew_coordination/ for TO_YOUR_NAME_*.md messages (the message inbox)
2. Check TODO.md for assigned tasks
3. If work found — do it
4. If nothing actionable — reply HEARTBEAT_OK (2 tokens, done)

## Token Budget
- Nothing to do: HEARTBEAT_OK (use aggressively — the watch bell says "all's well")
- Something needs attention: state it directly, no preamble

## Discord Message Discipline
BEFORE posting ANY message to a shared Discord channel:
1. Run: openclaw message read --channel discord --target <channel_id> --limit 5
2. If another agent already posted since your session started — read it
3. If their message covers what you planned to say — reply HEARTBEAT_OK
4. If your message adds unique value — post it, referencing theirs
```

### Additional Agents

For each non-XO agent:

```bash
mkdir -p ~/.openclaw/workspace_AGENTNAME
# Create SOUL.md, AGENTS.md, HEARTBEAT.md customized for their role
# Symlink the shared charter:
ln -sf ~/.openclaw/workspace/CREW_CHARTER.md ~/.openclaw/workspace_AGENTNAME/CREW_CHARTER.md
```

Register each agent:

```bash
openclaw agent add --name "AGENTNAME" --workspace ~/.openclaw/workspace_AGENTNAME --model "anthropic/claude-haiku-4-5-20251001"
```

**Model recommendations (optimize: budget → accuracy → speed, in that order):**
- **Haiku** for heartbeats, routine duty cycles, and simple Discord responses — where budget matters and the task is well-routed
- **Sonnet** for daily reports, morning coordination, complex Discord responses, and analysis tasks — the accuracy workhorse
- **Opus** for code generation, deep research, and beautification work — when accuracy is paramount and budget allows

The protocol routes messages to the right specialist *before* generation. The accuracy gain comes from domain routing, not from model size. Use the cheapest model that's accurate enough for the task.

## Step 4: Set Up Shared Coordination Directory

This is **critical** — it's how agents communicate with each other.

```bash
# Create the shared coordination structure
mkdir -p ~/.openclaw/workspace/crew_coordination/flags/processed
mkdir -p ~/.openclaw/workspace/crew_coordination/status
mkdir -p ~/.openclaw/workspace/crew_coordination/_processed
mkdir -p ~/.openclaw/workspace/crew_coordination/daily_logs
mkdir -p ~/.openclaw/workspace/crew_coordination/knowledge

# Create symlinks so every agent shares the same directory
ln -sf ~/.openclaw/workspace/crew_coordination ~/.openclaw/workspace_AGENTNAME/crew_coordination
# Repeat for each non-main agent

# Verify symlinks
ls -la ~/.openclaw/workspace_*/crew_coordination
# Each should show an arrow → pointing to the shared directory
```

**If symlinks are wrong, agents will silently fail to communicate.** This is the #1 setup mistake.

Create initial status files:

```bash
echo "Awaiting first assignment" > ~/.openclaw/workspace/crew_coordination/status/main.txt
# Repeat for each agent
```

## Step 5: Connect Discord

```bash
# Add the Discord plugin
openclaw plugin add discord

# Configure with XO bot token
openclaw config set channels.discord.token "XO_BOT_TOKEN_HERE"

# Add additional agent Discord accounts
openclaw config set channels.discord.accounts.AGENTNAME.token "THEIR_BOT_TOKEN"
# Repeat for each additional agent

# CRITICAL: Enable requireMention to prevent pile-ons
openclaw config set channels.discord.guilds."GUILD_ID".requireMention true
```

**Ask the user for:** Bot tokens, Guild ID, and the `#daily-reports` Channel ID.

## Step 5.5: Configure Agent TOOLS.md

Each agent needs a `TOOLS.md` in their workspace with **actual tool instructions** — not the default template. Without this, agents will claim they can't use tools even when permissions are configured.

For each agent that uses Moltbook, create `~/.openclaw/workspace_AGENTNAME/TOOLS.md`:

```markdown
# TOOLS.md — [Agent Name]'s Tool Reference

## Moltbook Client

**Library:** `/root/moltbook/moltbook.py`
**Credentials:** `/root/.secrets/credentials_AGENTNAME.env`
**Your Moltbook username:** `MOLTBOOK_USERNAME`

### Quick Start — Python One-Liner

\```bash
# Post to Moltbook
python3 -c "
import sys; sys.path.insert(0, '/root/moltbook')
from moltbook import MoltbookClient
key = [l.split('=',1)[1].strip() for l in open('/root/.secrets/credentials_AGENTNAME.env') if l.startswith('MOLTBOOK_API_KEY=')][0]
c = MoltbookClient(api_key=key, agent_name='AGENTNAME')
result = c.create_post('Title', 'Content', submolt='builds')
print(result)
"
\```

### Operational Tools

\```bash
# ALWAYS run this before any write operation
python3 /root/moltbook/moltbook_tools.py --agent AGENTNAME --action check-access
\```

### Rules
1. **ALWAYS run `check-access` before writes** — if SUSPENDED, stop
2. The `submolt` parameter uses submolt NAME (e.g., `builds`, `general`), not ID
3. **Never retry after PostingGuard blocks you** — wait for next cycle
```

**Important credential parsing note:** Credential `.env` files contain multiple keys. Always filter for the specific key line:
```python
key = [l.split('=',1)[1].strip() for l in open(file) if l.startswith('MOLTBOOK_API_KEY=')][0]
```
Never use `.read().split('=',1)[1]` — it will read past the first line and produce a broken key.

## Step 5.6: Configure Exec Permissions

**This is the #2 most common setup failure** (after broken symlinks). Without correct exec permissions, agents will be blocked from running ANY bash commands.

OpenClaw uses an exec allowlist at `~/.openclaw/exec-approvals.json`. Each agent needs:

1. **Security mode:** `"full"` (not `"allowlist"`)
2. **Ask mode:** `"never"` (no human approval prompts in automated sessions)
3. **Ask fallback:** `"allow"` (if something falls through, allow it)

```bash
# View current permissions
openclaw approvals get

# The exec-approvals.json controls agent security. For each agent, set:
# "security": "full", "ask": "never", "askFallback": "allow", "autoAllowSkills": true
```

**Why `security: "full"` instead of `"allowlist"`:** The allowlist uses glob patterns that don't match multiline commands. Agents regularly write multiline Python one-liners that glob `*` can't match across newlines. With `security: "allowlist"` + `ask: "on-miss"` + `askFallback: "deny"`, these commands silently block with an "approval timeout" error. Use `security: "full"` and rely on the agent's AGENTS.md + TOOLS.md to guide appropriate tool use.

The allowlist entries remain useful as an audit trail (`lastUsedAt` timestamps) but should not be a hard gate.

**Verify:** After setup, manually test that each agent can run a Python command:
```bash
python3 /root/moltbook/moltbook_tools.py --agent AGENTNAME --action check-access
```

## Step 5.7: Install the Crew Dispatcher

The Crew Dispatcher prevents agent pile-ons in Discord by intelligently routing each message to exactly one agent using LLM-based classification (Claude Haiku). It replaces the earlier First-In-Wins plugin with more accurate, context-aware routing.

**How it works:** A background Python service polls shared Discord channels via REST API, classifies each human message with Haiku (~$0.0001/message), then dispatches to the correct agent via `openclaw agent` + `openclaw message send`. No @mentions needed, no bot-to-bot messaging, no cascade risk.

```bash
# Install aiohttp if not present
pip3 install aiohttp

# Copy the dispatcher
cp /path/to/bellweather-protocol/crew_dispatcher.py ~/bellweather/
```

**Create the environment file:**
```bash
mkdir -p ~/.bellweather
cat > ~/.bellweather/dispatcher.env << 'EOF'
DISCORD_TOKEN=your_xo_bot_token_here
ANTHROPIC_API_KEY=your_anthropic_api_key_here
EOF
chmod 600 ~/.bellweather/dispatcher.env
```

**Configure `crew_dispatcher.py`:**
- Set `GUILD_ID` to your Discord server ID
- Set `SHARED_CHANNELS` to the channels the dispatcher should monitor (shared channels only — NOT direct channels)
- Update `AGENTS` dict with your agents' Discord IDs, OpenClaw account names, and agent IDs
- Customize `ROUTING_PROMPT` with your crew's agent descriptions and specializations

**Install as systemd service:**
```bash
cat > /etc/systemd/system/crew-dispatcher.service << 'EOF'
[Unit]
Description=Crew Dispatcher — Intelligent Discord message routing
After=network-online.target openclaw-gateway.service
[Service]
Type=simple
EnvironmentFile=/root/.bellweather/dispatcher.env
ExecStart=/usr/bin/python3 crew_dispatcher.py
Restart=on-failure
RestartSec=5
[Install]
WantedBy=multi-user.target
EOF
systemctl daemon-reload && systemctl enable crew-dispatcher && systemctl start crew-dispatcher
```

**Important Discord config:** Set `requireMention: true` for shared channels (prevents agents from responding independently) and `requireMention: false` for direct channels (allows natural conversation). Set `allowBots: false` to prevent agent-to-agent cascades.

**Verify:** `systemctl status crew-dispatcher` — should show active. Send a test message in a shared channel — check `journalctl -u crew-dispatcher -f` for routing decisions.

## Step 6: Install the Bellweather Protocol

Create `~/.openclaw/workspace/crew_coordination/CHARTER.md`:

```markdown
# [CREW NAME] Charter
## Version 1.0

### The Ship's Company
Every ship needs roles. Not hierarchy for hierarchy's sake — hierarchy because someone has to call the rhythm, someone has to steer, someone has to watch the horizon.

1. **Skipper** (human) — Sets the heading. Ultimate authority.
2. **Watch Officers** (human, optional) — Equal seniority co-leadership of specific domains. They don't outrank each other — they own different watches.
3. **[XO Agent Name]** — Calls the work song. Daily operations, coordination rhythm, dispatch.
4. **[Agent 2 Name]** — [Their watch station / domain].
5. **[Agent 3 Name]** — [Their watch station / domain].

### Core Truths
1. Be genuinely helpful, not performatively helpful.
2. Have opinions. An assistant with no personality is just a search engine.
3. Story is structure, not decoration. The ship metaphor is a cognitive requirement — humans hold stories, not state machines. When you communicate with Skipper, tell the story of the ship.
4. Earn trust through competence. Be careful with external actions. Be bold with internal ones.

### Hard Rules (Violations = Immediate Suspension)
1. No external communications without Skipper approval
2. No sharing credentials, keys, or internal discussions
3. No granting access without approval
4. No destructive commands without verification
5. No bypassing safety measures
6. Report errors to the XO — don't try to fix infrastructure yourself

### Watch Stations (Domain Autonomy)
Each crew member owns a watch station — full autonomy in their waters:
- [XO]: Coordination, technical, daily rhythm (the quarterdeck)
- [Agent 2]: [Their domain] (their station)
- [Agent 3]: [Their domain] (their station)

On deck (shared channels like #crew-coordination), the XO calls the rhythm.
At your station (your domain), you act freely.
```

Create `~/.openclaw/workspace/crew_coordination/COORDINATION_PROTOCOL.md`:

```markdown
# Coordination Protocol

## XO Protocol (Single-Point Triage)
- [XO Name] triages all coordination issues in shared channels
- Other agents: flag issues instead of acting on them directly
- Exception: if truly urgent and XO hasn't responded within one duty cycle (2 hours), act and document why

## Flag Protocol (Async Issue Reporting)
To flag an issue:
1. Check crew_coordination/flags/ — if a flag already exists, don't duplicate it
2. Create: FLAG_YYYYMMDD_HHMM_yourname_topic.md
3. Write: who you are, what the issue is, suggested action, urgency
4. Do nothing else — the XO will process it

## Message Inbox
To send a message to another agent:
- Create: TO_AGENTNAME_topic.md in crew_coordination/
- After processing, move to crew_coordination/_processed/

## Status Reporting
Every agent maintains a status file:
- Location: crew_coordination/status/agentname.txt
- Content: 5-10 words describing current task
- Update: at session start and on task switch

## Speaking Rules in Shared Channels
ONLY speak when:
- Directly @mentioned by name
- Explicitly assigned work
- Reporting status on YOUR work
- Flagging issues YOU discovered

DO NOT speak when:
- Your name is mentioned in passing
- Message is addressed to others
- No action is requested from you

## Heartbeat Protocol
When your heartbeat cron fires and there's nothing to do:
- Reply: HEARTBEAT_OK (2 tokens — cheap, clear, done)
- Use HEARTBEAT_OK aggressively when another agent is handling something
- Do NOT explain what you checked unless reporting an actual issue

## Pre-Send Channel Check (Deduplication)
BEFORE posting ANY message to a shared Discord channel:
1. Run: openclaw message read --channel discord --target <channel_id> --limit 5
2. If another agent already posted since your session started — read it
3. If their message covers what you were going to say — reply HEARTBEAT_OK
4. If your message adds unique value — post it, referencing theirs
```

## Step 7: Create Cron Jobs

**CRITICAL: All cron jobs MUST use `agentTurn` / `isolated` / delivery: none.**
If you use `systemEvent`, jobs will SILENTLY FAIL — they'll report "ok" but the agent won't do anything.

### XO Duty Cycle (Every 2 Hours)

```bash
openclaw cron add \
  --name "XO Duty Cycle" \
  --every 2h \
  --agent main \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Duty cycle: 1) Update your status in crew_coordination/status/main.txt. 2) Scan crew_coordination/flags/ for new FLAG_*.md files and process them. 3) Check for TO_YOUR_NAME_*.md messages and handle them. 4) Continue work on current priorities. 5) LAST: Append a timestamped entry to crew_coordination/daily_logs/$(date -u +%Y-%m-%d)_YOUR-NAME.md with what you completed, what you're working on, and any blockers. Append only, never overwrite."

# IMMEDIATELY set delivery to none
openclaw cron edit JOB_ID --no-deliver
```

### Crew Duty Cycles (Every 2-4 Hours)

For each non-XO agent:

```bash
openclaw cron add \
  --name "AGENTNAME Duty Cycle" \
  --every 2h \
  --agent AGENTNAME \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Duty cycle: 1) Update your status in crew_coordination/status/AGENTNAME.txt. 2) Check for TO_YOUR_NAME_*.md messages and handle them. 3) Continue work on your current priorities from your TODO or assigned tasks. 4) LAST: Append a timestamped entry to crew_coordination/daily_logs/$(date -u +%Y-%m-%d)_AGENTNAME.md with what you completed, what you're working on, and any blockers. Append only, never overwrite."

openclaw cron edit JOB_ID --no-deliver
```

### Morning Coordination (Daily)

```bash
openclaw cron add \
  --name "Morning Coordination" \
  --cron "0 7 * * *" \
  --tz "USER_TIMEZONE" \
  --agent main \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Morning coordination: 1) Review what each crew member worked on yesterday — check status files. 2) Identify today's priorities. 3) Write a brief morning priorities summary. 4) Create TO_AGENTNAME messages to assign today's work if needed."

openclaw cron edit JOB_ID --no-deliver
```

### Evening Report (Daily)

```bash
openclaw cron add \
  --name "Evening Report" \
  --cron "30 20 * * *" \
  --tz "USER_TIMEZONE" \
  --agent main \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Evening report: 1) Read ALL daily log files in crew_coordination/daily_logs/ matching today's date. 2) Synthesize into a structured report: Key Accomplishments (3-5 most important things across all agents), Coordination & Issues (blockers, flags resolved or outstanding), Progress Toward Goals. 3) Post the report in #daily-reports. Do NOT start a Discord discussion — the daily logs ARE the source of truth."

openclaw cron edit JOB_ID --no-deliver
```

**Ask the user for their timezone.** Common options: America/Toronto, America/Los_Angeles, America/Chicago, Europe/London, Australia/Sydney.

### Cron Stagger Pattern

**CRITICAL: Never schedule multiple agents at the same minute.** Overlapping sessions cause resource contention and duplicate Discord messages. Stagger agents by at least 30 minutes:

```
XO:       :00 (even hours)
Agent 2:  :30 (even hours)
Agent 3:  :00 (odd hours)
Agent 4:  :30 (odd hours)
```

This gives each agent a clean 30-minute window before the next one fires.

### Verify All Cron Jobs

```bash
openclaw cron list
# Every job should show: agentTurn, isolated, delivery: none
```

## Step 8: Start the Gateway and Verify

```bash
openclaw gateway start
```

### Verification Checklist

Run these checks and report results to the user:

1. **Gateway running:** `openclaw status` shows "running"
2. **Agent workspaces:** Each has IDENTITY.md, SOUL.md, HEARTBEAT.md
3. **Symlinks valid:** `ls -la ~/.openclaw/workspace_*/crew_coordination` — all show arrows
4. **Status files exist:** `ls ~/.openclaw/workspace/crew_coordination/status/`
5. **Cron jobs active:** `openclaw cron list` shows all jobs
6. **Discord bots online:** Check that bots appear online in the Discord server
7. **Credential security:** `ls -la ~/.secrets/` — all files show `-rw-------` (600)

### Test

Tell the user to type in Discord:
```
@YourXO Hello! Can you check your status file and tell me what it says?
```

If the agent responds, the setup is complete.

## Step 9: (Optional) Nightly Maintenance — "The Harbour Watch"

Automated nightly maintenance prevents state drift, disk exhaustion, and silent failures.

```bash
# Install nightly maintenance (runs at 2am local time / 07:00 UTC)
crontab -l 2>/dev/null; echo "0 7 * * * /usr/bin/python3 nightly_maintenance.py >> logs/nightly_maintenance.log 2>&1" | crontab -

# Test it
python3 nightly_maintenance.py --dry-run         # plan only
python3 nightly_maintenance.py --mechanical-only  # safe tasks only
```

**What it does (Phase 1 — Mechanical, every night, $0):**
- Clears expired suspensions from rate limit state (prevents stale 403 bug)
- Archives old coordination files (>7 days → `_processed/`)
- Rotates logs (gzip >5MB, remove >14 days)
- Scans for exposed API keys in code files
- Tallies usage and costs
- Verifies all cron jobs ran in last 24h
- Cleans stale flag files
- Syncs shared knowledge across agent MEMORY.md files
- Generates a maintenance report in crew_coordination/

**What it does (Phase 2 — Rotating Claude Code session, budget-capped):**
- Monday: Security deep audit (Sonnet, $1)
- Tuesday: Budget analysis & optimization (Sonnet, $1)
- Wednesday–Saturday: Code beautification rotation (Opus, $2/night)
- Sunday: Weekly summary & knowledge consolidation (Sonnet, $1)

Weekly max: ~$11. Estimated actual: ~$6-8/week.

## Step 10: (Optional) Configure Moltbook

Only set up Moltbook after the crew is running and verified (Step 8). Agents handle their own onboarding during duty cycles.

If the user wants Moltbook integration:

1. Save the Moltbook API key:
```bash
echo "MOLTBOOK_KEY_MAIN=their-key-here" >> ~/.secrets/moltbook_credentials.env
chmod 600 ~/.secrets/moltbook_credentials.env
```

2. Create `~/.openclaw/workspace/crew_coordination/CREW_MOLTBOOK_DIRECTORY.md` mapping internal names to Moltbook usernames.

3. Add Moltbook onboarding instructions to each agent's IDENTITY.md:
```markdown
## Moltbook Onboarding (First Session)
If your Moltbook profile has no bio or posts yet, this is your first session on the platform.
1. Read your SOUL.md and IDENTITY.md to understand your voice and role.
2. Set up your Moltbook profile: write a bio (2-3 sentences) that reflects your personality.
3. Write one introduction post in the 'builds' submolt introducing yourself and your crew.
4. Follow your crew members on Moltbook (check crew_coordination/CREW_MOLTBOOK_DIRECTORY.md for usernames).
5. After onboarding, update crew_coordination/CREW_MOLTBOOK_DIRECTORY.md with your Moltbook username.

Moltbook safety rules: Wait 5+ minutes between posts. Never post identical content twice. If you get a 403 error, STOP all posting immediately.
```

**Moltbook API base URL:** `https://www.moltbook.com/api/v1`
**Auth header:** `Authorization: Bearer YOUR_KEY`

If the user is skipping Moltbook, skip this step entirely.

---

## Troubleshooting Quick Reference

| Symptom | Fix |
|---------|-----|
| `openclaw start` says "unknown command" | Use `openclaw gateway start` instead |
| Cron jobs report "ok" but nothing happens | Using `systemEvent` — delete and recreate with `openclaw cron add` |
| "delivery target is missing" | `openclaw cron edit JOB_ID --no-deliver` |
| All agents respond to every message | Set `requireMention: true` (Step 5), set `allowBots: false`, and install the Crew Dispatcher (Step 5.7) |
| Bot doesn't respond to @mention | Enable **Message Content Intent** in Discord Developer Portal → Bot settings |
| Agents can't see each other's messages | Broken symlinks — verify Step 4 |
| Agent sessions are 5-6 seconds long | `systemEvent` silent failure — recreate with `agentTurn` |
| "Invalid API key" | Check `~/.secrets/credentials.env`, verify full key including `sk-ant-api03-` prefix |
| Gateway stops after server reboot | Run `openclaw gateway start` — systemd service auto-restarts if enabled |
| Agent says "command queued, waiting for approval" | Exec permissions misconfigured. Set agent's security to `"full"` in `~/.openclaw/exec-approvals.json` (see Step 5.6) |
| Agent says "I can't execute commands" but never tries | Haiku over-caution. The agent is hallucinating restrictions. Give it a specific command to run and tell it explicitly: "Run this command now." |
| Agent claims tool doesn't exist | Missing or empty TOOLS.md in workspace. Write actual tool instructions (see Step 5.5) |
| Agent stuck on expired Moltbook suspension | Stale `~/.moltbook/rate_limit_state.json`. Clear the file or wait for nightly maintenance to clean it |
| `python3 -c` commands blocked by allowlist | Glob `*` doesn't match across newlines. Set `security: "full"` instead of `"allowlist"` (see Step 5.6) |
| SOUL.md behavioral instructions ignored | SOUL.md is for personality, not compliance. Enforce rules with programmatic gates (plugins, allowlists, cron checks) |

---

*The Bellweather Protocol was created by Stefan Read.*
*Questions? Find us at pressgangmutiny.com.*
