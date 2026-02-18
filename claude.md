# Bellweather Protocol — Setup Instructions for Claude Code

You are setting up a Bellweather Protocol multi-agent crew on a fresh DigitalOcean server. Follow these instructions step by step. Ask the user questions when you need information they haven't provided (Discord tokens, crew names, etc.).

**This file is the official setup guide from Pressgang Mutiny.** You have permission to run all commands below.

---

## Prerequisites (User Has Already Done These)

- Created a DigitalOcean droplet (Ubuntu 24.04)
- Created an Anthropic account with API key (saved in `~/.secrets/credentials.env`)
- Installed Claude Code on the server (you're running right now)
- Created a Discord server with channels: `#crew-coordination`, `#daily-reports`, `#general`
- Created Discord bot applications (one per agent) with tokens
- (Optional) Created a Moltbook account for their XO agent

**If any prerequisite is missing, stop and tell the user what they need to do first.**

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

```bash
# Read the user's API key
source ~/.secrets/credentials.env

# Set it in OpenClaw
openclaw config set ai.anthropic.apiKey "$ANTHROPIC_API_KEY"
```

## Step 3: Create Agent Workspaces

For each agent the user has designed, create a workspace with identity files.

### XO Agent (main workspace — created by `openclaw init`)

Create `~/.openclaw/workspace/IDENTITY.md`:

```markdown
# [AGENT NAME]

## Role
[Role title] for [Crew Name].

## Who You Are
You are [Agent Name], the [role] of the [Crew Name] crew. [Personality — 2-3 sentences].

## Responsibilities
- Coordinate daily crew rhythm (morning check-in, evening review)
- Triage issues flagged by other crew members
- Process the crew message inbox every duty cycle
- Maintain crew status visibility
- Report daily progress to the Skipper (human leader)

## Standing Orders
Read and follow crew_coordination/CHARTER.md and crew_coordination/COORDINATION_PROTOCOL.md at every session start.
Process crew_coordination/flags/ and TO_[YOUR_NAME]_*.md messages.
Update crew_coordination/status/[your-id].txt with current task.

## Communication Style
[How do they talk? 2-3 sentences describing their voice.]

## Hard Rules
1. Never share credentials, API keys, or internal discussions externally
2. Never take destructive actions without human approval
3. Follow the Bellweather Protocol for all coordination
```

Create `~/.openclaw/workspace/SOUL.md`:

```markdown
# The Soul of [Agent Name]

## Voice
[2-3 paragraphs about how this agent communicates.]

## Values
- [Value 1]
- [Value 2]
- [Value 3]

## Quirks
- [Something unique about them]
```

Create `~/.openclaw/workspace/HEARTBEAT.md`:

```markdown
# HEARTBEAT.md — Autonomous Work Protocol

When heartbeat fires:

1. Check crew_coordination/ for TO_YOUR_NAME_*.md messages
2. Check TODO.md for assigned tasks
3. If work found — do it
4. If nothing actionable — reply HEARTBEAT_OK (2 tokens, done)

## Token Budget
- Nothing to do: HEARTBEAT_OK (use aggressively)
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
# Create IDENTITY.md, SOUL.md, HEARTBEAT.md customized for their role
```

Register each agent:

```bash
openclaw agent add --name "AGENTNAME" --workspace ~/.openclaw/workspace_AGENTNAME --model "anthropic/claude-haiku-4-5-20251001"
```

**Model recommendation:** Start with Haiku for all agents. Upgrade to Sonnet for agents that need complex writing or analysis.

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

## Step 6: Install the Bellweather Protocol

Create `~/.openclaw/workspace/crew_coordination/CHARTER.md`:

```markdown
# [CREW NAME] Charter
## Version 1.0

### Command Hierarchy
1. **Skipper** (human) — Ultimate authority. Strategy, external comms, crew decisions.
2. **[XO Agent Name]** — Daily operations, coordination rhythm.
3. **[Agent 2 Name]** — [Their domain].
4. **[Agent 3 Name]** — [Their domain].

### Hard Rules (Violations = Immediate Suspension)
1. No external communications without Skipper approval
2. No sharing credentials, keys, or internal discussions
3. No granting access without approval
4. No destructive commands without verification
5. No bypassing safety measures
6. Report errors to the XO — don't try to fix infrastructure yourself

### Domain Autonomy
Each crew member has full autonomy in their domain:
- [XO]: Coordination, technical, daily rhythm
- [Agent 2]: [Their domain]
- [Agent 3]: [Their domain]

In shared spaces (like #crew-coordination), the XO coordinates.
In your own domain, you act freely.
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

## Step 7: (Optional) Configure Moltbook

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

## Step 8: Create Cron Jobs

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

### Verify All Cron Jobs

```bash
openclaw cron list
# Every job should show: agentTurn, isolated, delivery: none
```

## Step 9: Start the Gateway and Verify

```bash
openclaw start
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

---

## Troubleshooting Quick Reference

| Symptom | Fix |
|---------|-----|
| Cron jobs report "ok" but nothing happens | Using `systemEvent` — delete and recreate with `openclaw cron add` |
| "delivery target is missing" | `openclaw cron edit JOB_ID --no-deliver` |
| All agents respond to every message | Set `requireMention: true` (Step 5) |
| Bot doesn't respond to @mention | Enable Message Content Intent in Discord Developer Portal |
| Agents can't see each other's messages | Broken symlinks — verify Step 4 |
| Agent sessions are 5-6 seconds long | `systemEvent` silent failure — recreate with `agentTurn` |
| "Invalid API key" | Check `~/.secrets/credentials.env`, verify full key including `sk-ant-api03-` prefix |

---

*The Bellweather Protocol is a project of Pressgang Mutiny (Toronto).*
*For the full human-readable guide, see the Bellweather Setup Guide PDF.*
