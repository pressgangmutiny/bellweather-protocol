<div class="title-page">
<img src="logo.png" alt="Bellweather Protocol" class="title-logo" />
<h1>The Bellweather Setup Guide</h1>
<h2>How to Build Your Own AI Crew from Scratch</h2>
<p><strong>A Step-by-Step Guide for Non-Technical People</strong></p>
<p><strong>Version 0.4 — February 2026</strong></p>
<p><em>By Pressgang Mutiny (Toronto) &amp; The Bellweather Crew</em></p>
</div>

---

## The Problem We're Solving

Every organization building with AI agents will face the same coordination paradox: **How do you get humans to work WITH specialized AI agents instead of defending against them or abdicating control entirely?**

Most solutions are technical-only — better APIs, smarter routing, tighter permissions. They fail because they ignore the human half of the equation. Humans who perceive AI as a threat will delegate guardedly, withhold context, and second-guess every output. Humans who abdicate control will over-delegate, lose situational awareness, and blame agents when things go wrong. Neither pattern leads to effective coordination.

**The Bellweather Protocol solves this with structured coordination:** An XO protocol, flag system, and structured daily rhythm keep agents synchronized without stepping on each other. Role clarity, clear delegation, and heartbeat protocols prevent the pile-ons and duplications that plague naive multi-agent systems.

**Why this matters:** Every tech company building agent swarms will face this coordination problem within five years. The Bellweather Protocol is a battle-tested framework — proven in production with 4 AI agents coordinating across Discord, Google Workspace, multiple API integrations, and a shared filesystem.

---

## What You'll Get

By the end of this guide, you will have:

- **A team of AI agents** (2-4 agents) running on your own server, 24/7
- **A coordination system** (the Bellweather Protocol) that keeps your agents organized and working together
- **A Discord server** where you and your agents communicate naturally
- **(Optional) Social media presence** for your agents on Moltbook (the agent social network)
- **Automated scheduling** so your agents check in, coordinate, and report to you daily

**This is a real production system.** Pressgang Mutiny runs 4 AI agents coordinating across Discord, WhatsApp, Moltbook, Google Workspace, and multiple API integrations using this exact setup. The agents handle community engagement, logistics, creative work, and technical coordination — all governed by the Bellweather Protocol.

### Choose Your Path

This guide offers **two ways** to set up your crew:

| Path | Who It's For | Time | What You Do |
|------|-------------|------|-------------|
| **Path A: Easy Mode** | Anyone with a Claude Code subscription | ~1 hour of your time | Set up server + Claude Code, then Claude does the rest |
| **Path B: Manual Setup** | People who want to understand every detail | 5-7 hours | Follow each step yourself, copy-paste commands |

**Path A is recommended for most people.** You still control everything — Claude just handles the tedious command-line work. You make all the creative decisions (crew names, roles, personalities).

**Path B is the original guide** — every command explained, every concept covered. Use it if you want to learn the system inside-out, or if you don't have a Claude Code subscription.

Both paths produce the same result. You can switch from Path A to Path B at any point.

---

### Time Estimate

**Path A: Easy Mode**

| Phase | Time | What You're Doing |
|-------|------|-------------------|
| Server + accounts | 30-45 min | Creating accounts, getting a server (you do this) |
| Design your crew | 15-20 min | Choosing names, roles, personalities (the fun part) |
| Let Claude work | 30-60 min | Claude sets everything up while you watch |
| Verify | 10 min | Check that everything works |
| **Total** | **~1.5-2 hours** | **Most of it is waiting for accounts to activate** |

**Path B: Manual Setup**

| Phase | Time | What You're Doing |
|-------|------|-------------------|
| Steps 1-3 | 1-2 hours | Setting up server and AI access |
| Step 4 | 30 min | Designing your crew (the fun part) |
| Steps 5-6 | 1-2 hours | Creating agents, connecting Discord |
| Steps 7-8 | 1 hour | Social network + protocol setup |
| Steps 9-10 | 1 hour | Automation and verification |
| **Total** | **5-7 hours** | **Spread across 1-2 days is comfortable** |

You don't need to do this all in one sitting. Each step saves your progress.

---

## Budget Breakdown

Everything runs on monthly subscriptions. No long-term contracts. Cancel anytime.

### Tier 1: Minimal (~$48 CAD / ~$34 USD per month)

Good for: Testing the concept, running 1-2 agents with light usage.

| Service | Monthly Cost (CAD / USD) | What It's For |
|---------|--------------------------|---------------|
| DigitalOcean Server | $34 / $24 | Your server (4 GB RAM droplet) |
| Anthropic API Tokens | $14 / $10 | AI "brain" for your agents |
| Discord | Free | Communication channel |
| Moltbook (optional) | Free | Agent social network |
| **Total** | **~$48 / ~$34** | |

### Tier 2: Recommended (~$172 CAD / ~$123 USD per month)

Good for: A proper crew of 3-4 agents with moderate daily activity.

| Service | Monthly Cost (CAD / USD) | What It's For |
|---------|--------------------------|---------------|
| DigitalOcean Server | $88 / $63 | Bigger server (8 GB RAM, 4 CPUs) |
| Claude Code Subscription | $28 / $20 | Pro tier — easier setup & debugging |
| Anthropic API Tokens | $56 / $40 | More AI usage for busy agents |
| Discord | Free | Communication channel |
| Moltbook (optional) | Free | Agent social network |
| **Total** | **~$172 / ~$123** | |

### Tier 3: Full Production (~$431 CAD / ~$308 USD per month)

Good for: Heavy daily usage, 4+ agents, maximum capability.

| Service | Monthly Cost (CAD / USD) | What It's For |
|---------|--------------------------|---------------|
| DigitalOcean Server | $88 / $63 | 8 GB RAM, 4 CPUs |
| Claude Code Subscription | $280 / $200 | Max tier — highest usage limits |
| Anthropic API Tokens | $56 / $40 | Heavy API usage |
| Discord | Free | Communication channel |
| Moltbook (optional) | Free | Agent social network |
| **Total** | **~$424 / ~$303** | |

**Note:** All CAD prices use an approximate 1.40 CAD/USD exchange rate. Check current rates for exact pricing.

**Start with Tier 1.** You can always upgrade later. The server can be resized without losing data, and API usage is pay-as-you-go.

---

## Prerequisites Checklist

Before you begin, make sure you have:

- [ ] A computer with internet access (Mac, Windows, or Linux — any will work)
- [ ] A credit card or debit card (for DigitalOcean and Anthropic signups)
- [ ] An email address
- [ ] A Discord account (free — sign up at https://discord.com if you don't have one)
- [ ] About 1.5-2 hours (Path A) or 5-7 hours (Path B) spread across 1-2 days
- [ ] A name for your crew (you'll need this in Step 4)

**That's it.** No programming experience required. Path A lets Claude handle the technical parts. Path B has every command as copy-paste.

---

# Path A: Easy Mode (Claude Does the Work)

**What you're doing:** Setting up the minimum infrastructure yourself, then letting Claude Code handle the rest of the setup by reading this guide.

**Why this works:** Claude Code is an AI assistant that can run commands on your server, create files, configure software, and troubleshoot problems — exactly what the manual steps require. Instead of you copying and pasting 100+ commands, Claude reads this guide and executes them for you, asking you questions along the way.

**What you still need to do yourself:** Create accounts (DigitalOcean, Anthropic, Discord) — Claude can't sign up for services on your behalf. You also make all the creative decisions: crew name, agent names, roles, and personalities.

**Time:** ~1.5-2 hours total (much of it is waiting for accounts)

---

## A.1 Create Your Accounts

You need to create these accounts before Claude can help. Do them in parallel while things load:

### DigitalOcean (your server)

1. Go to **https://cloud.digitalocean.com/registrations/new**
2. Sign up with your email, Google account, or GitHub account
3. Add a payment method (credit card or PayPal)
4. Create a Droplet:
   - **Image:** Ubuntu 24.04 (LTS)
   - **Size:** Basic → Regular → **$24/mo** (4 GB RAM) to start. $63/mo (8 GB) if you want headroom.
   - **Region:** Closest to you
   - **Authentication:** Password (pick something strong, write it down)
   - **Hostname:** Something memorable (e.g., `bellweather-crew`)
5. Click **Create Droplet** and write down the **IP address** when it appears.

### Anthropic (your AI brain)

1. Go to **https://console.anthropic.com/**
2. Sign up and verify your email
3. Go to **Settings → Billing**, add a payment method
4. Set a usage limit ($10-20 USD/month to start)
5. Go to **Settings → API Keys**, click **Create Key**
6. Name it "Bellweather Crew" and **copy the key immediately** (starts with `sk-ant-api03-...`)
7. Save this key somewhere safe — you won't see it again

### Claude Code Subscription

1. Go to **https://claude.ai/pricing**
2. Subscribe to **Pro** ($20 USD/month) or **Max** ($200 USD/month)
3. This gives you direct access to Claude Code on your server

### Discord (your communication channel)

1. If you don't have Discord: sign up at **https://discord.com**
2. Create a new server: click **+** → **Create My Own** → **For me and my friends**
3. Name it after your crew (e.g., "Bellweather Crew")
4. Create these channels: `#crew-coordination`, `#daily-reports`, `#general`
5. For each agent you plan to have (2-4), create a Discord Bot:
   - Go to **https://discord.com/developers/applications**
   - Click **New Application**, name it after your agent
   - Go to **Bot** → **Reset Token** → copy and save the token
   - Enable all three **Privileged Gateway Intents**
   - Go to **OAuth2** → **URL Generator** → check `bot` → check Send/Read Messages, Read History, Embed Links, Attach Files
   - Copy the URL, paste in browser, invite to your server

### Moltbook (optional — agent social network)

Moltbook is a social network for AI agents. It gives your crew a public presence where they can interact with other agents and humans. **This is entirely optional** — your crew will work fine without it.

If you want to use Moltbook:

1. Go to **https://www.moltbook.com/**
2. Create **one** account — this will be your crew's first agent (your XO/coordinator)
3. Go to Settings → API/Developer → Generate an API key
4. Save the key

**Don't create accounts for every agent yet.** Registering bot accounts before the agents exist on your server can cause problems — the login flow expects an agent that's already configured. In Step A.3, Claude will set up your XO agent first, then use it to register the rest of the crew automatically.

If you're skipping Moltbook, just leave the Moltbook line in the handoff prompt as "not using Moltbook" and skip Step 7.

---

## A.2 Install Claude Code on Your Server

Connect to your server and install Claude Code:

**On Mac:** Open Terminal (Cmd+Space, type "Terminal")
**On Windows:** Open PowerShell (Windows key, type "PowerShell")

```
ssh root@YOUR_IP
```

Type `yes` when asked about the fingerprint, enter your password.

Now run these commands one at a time:

```
apt update && apt upgrade -y
```

```
apt install -y curl git build-essential python3 python3-pip
```

Install Node.js:

```
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
```

```
apt install -y nodejs
```

Install Claude Code:

```
npm install -g @anthropic-ai/claude-code
```

Now authenticate Claude Code:

```
claude
```

This starts Claude Code. It will ask you to log in — follow the prompts to authenticate with your Claude subscription. Once you see the Claude Code prompt (a `>` where you can type), you're ready. **Press Ctrl+C to exit Claude Code for now** — you'll restart it in the next step after preparing the setup files.

> **⚠️ Watch for authentication loops.** If Claude Code fails to authenticate (wrong credentials, expired token, OAuth redirect issues), it may retry rapidly and burn through your API quota or system processes. If you see repeated error messages during login:
>
> 1. **Press Ctrl+C immediately** to stop Claude Code.
> 2. Check your Anthropic API key is correct.
> 3. If using OAuth, clear the cached token: `rm -rf ~/.claude/oauth*`
> 4. Try again with `claude` — if it fails a second time, check [status.anthropic.com](https://status.anthropic.com) before retrying.
>
> **Do not let it loop.** A stuck authentication retry can spawn dozens of background processes. If this happens, clean up with `pkill -f claude`, then fix the underlying issue before restarting.

---

## A.3 Hand Off to Claude

Now for the magic. You're going to download the Bellweather setup instructions and let Claude Code read them automatically.

**All commands in this section run in your regular terminal** (not inside Claude Code).

### Save your credentials

```
mkdir -p ~/.secrets
```

```
nano ~/.secrets/credentials.env
```

Add your Anthropic API key:

```
ANTHROPIC_API_KEY=sk-ant-api03-YOUR-KEY-HERE
```

Save (Ctrl+X, Y, Enter). Lock it:

```
chmod 600 ~/.secrets/credentials.env
```

### Download the setup instructions

This downloads a `CLAUDE.md` file — Claude Code's native instruction format. When Claude Code starts in a directory containing a `CLAUDE.md`, it reads the instructions automatically. No prompting needed.

```
mkdir -p ~/bellweather-setup
curl -o ~/bellweather-setup/CLAUDE.md https://raw.githubusercontent.com/pressgangmutiny/bellweather-protocol/main/claude.md
```

### Start Claude Code

```
cd ~/bellweather-setup
claude
```

Claude Code will automatically read the setup instructions. It will ask you for the information it needs. **Paste this to get started:**

```
I want to set up a Bellweather Protocol multi-agent crew on this server.

Here's what I've already done:
- Created a DigitalOcean server (you're running on it now)
- Created an Anthropic account with API key (saved in ~/.secrets/credentials.env)
- Created a Discord server with channels: #crew-coordination, #daily-reports, #general
- Created Discord bot applications with these tokens: [PASTE YOUR BOT TOKENS]
- My Discord Guild ID is: [PASTE YOUR GUILD ID]
- My #daily-reports Channel ID is: [PASTE CHANNEL ID]
- Moltbook: [PASTE API KEY if you have one, "not yet — please set this up for me", or "not using Moltbook"]

My crew design:
- Crew name: [YOUR CREW NAME]
- Agent 1 (XO): Name: [NAME], Role: [ROLE], Personality: [DESCRIPTION]
- Agent 2: Name: [NAME], Role: [ROLE], Personality: [DESCRIPTION]
- Agent 3: Name: [NAME], Role: [ROLE], Personality: [DESCRIPTION]
- (Add Agent 4 if you want one)

Please follow the CLAUDE.md instructions to set everything up.
Ask me questions if you need any information I haven't provided.
```

**Fill in the bracketed parts** with your actual information, then let Claude work. It will:

- Install OpenClaw and configure it
- Create all your agent workspaces and identity files
- Set up the coordination filesystem with proper symlinks
- Connect Discord and configure requireMention
- Set up Moltbook integration (if using Moltbook)
- Create all the cron jobs with the correct configuration
- Verify everything works

**Claude will ask you questions** when it needs decisions — like confirming agent personality details, choosing models (Haiku vs. Sonnet), or clarifying your preferences. Answer them and let it continue.

**This typically takes 30-60 minutes** of Claude working, during which you can watch the progress or go get coffee.

---

## A.4 Verify It's Working

Once Claude reports that setup is complete, verify:

1. **Discord:** Go to your Discord server. Type `@YourXO Hello! Can you check your status file and tell me what it says?` — you should get a response.

2. **Cron jobs:** Run `openclaw cron list` and verify all jobs show the correct schedule.

3. **Coordination:** Run `ls -la ~/.openclaw/workspace_*/crew_coordination` and verify all symlinks point to the shared directory.

4. **Moltbook (if using):** Check that your XO agent's profile exists at moltbook.com.

5. **Wait 24 hours:** Let the crew run for a day. Check that morning and evening reports appear in #daily-reports, status files are updating, and agents are responding to @mentions.

**If something's wrong,** tell Claude Code what's happening and it can diagnose and fix it. The Troubleshooting Reference at the end of this guide covers common issues.

---

**That's Path A complete.** Skip ahead to the Troubleshooting Reference if you need it, or read through Path B to understand what Claude set up for you.

---

# Path B: Manual Setup (Step by Step)

The following steps walk you through every detail of the setup process. If you used Path A, you can skip to the Troubleshooting Reference.

---

# Step 1: Get a Server

**What you're doing:** Renting a small computer in the cloud that will run your AI agents 24/7.

**Why:** Your agents need a computer that's always on. Your laptop sleeps, loses internet, runs out of battery. A cloud server runs forever (or until you stop paying for it).

**Time:** 15-30 minutes

---

### 1.1 Create a DigitalOcean Account

1. Go to **https://cloud.digitalocean.com/registrations/new**
2. Sign up with your email, Google account, or GitHub account
3. You'll need to add a payment method (credit card or PayPal)
4. Verify your email if prompted

**What you should see:** The DigitalOcean dashboard — a clean page with a green "Create" button in the top right.

### 1.2 Create a Droplet (Your Server)

A "Droplet" is DigitalOcean's name for a virtual server.

1. Click the green **Create** button in the top right
2. Select **Droplets**
3. Configure your droplet:

| Setting | What to Choose | Why |
|---------|---------------|-----|
| **Region** | The location closest to you (e.g., Toronto, New York, San Francisco) | Lower latency. Pick whatever is nearest. |
| **Image** | **Ubuntu 24.04 (LTS)** | This is a Linux operating system. LTS means "Long Term Support" — it gets security updates for years. |
| **Size** | **Basic → Regular → $24/mo** (4 GB RAM, 2 CPUs, 80 GB disk) for Tier 1, or **$63/mo** (8 GB RAM, 4 CPUs) for Tier 2/3 | This is the computer's power. 4 GB is fine to start. |
| **Authentication** | **Password** (simpler for beginners) | You'll use this to log into your server. Pick something strong. |

1. Under **Authentication**, choose **Password** and enter a strong password - Write this password down somewhere safe — you'll need it - Must be at least 8 characters with uppercase, lowercase, and a number
2. Under **Hostname**, give your server a name (e.g., `bellweather-crew`)
3. Click **Create Droplet**

**What you should see:** A progress bar, then your new droplet appears with an IP address (looks like `134.122.46.35`).

**Write down your IP address** — you'll use it throughout this guide. It's the address of your new server.

### 1.3 Connect to Your Server

Now you need to open a command line (terminal) and connect to your server.

**On Mac:** 1. Open **Terminal** (press Cmd+Space, type "Terminal", press Enter) 2. Type this command (replace `YOUR_IP` with your droplet's IP address):

```
ssh root@YOUR_IP
```

1. Type `yes` when it asks about the fingerprint
2. Enter the password you set in Step 1.2

**On Windows:** 1. Open **PowerShell** (press Windows key, type "PowerShell", press Enter) 2. Type this command:

```
ssh root@YOUR_IP
```

1. Type `yes` when it asks about the fingerprint
2. Enter the password you set in Step 1.2

**What you should see:** A command prompt that looks something like:

```
root@bellweather-crew:~#
```

Congratulations — you're now logged into your server! Everything you type here runs on that remote computer.

### 1.4 Quick Server Setup

Run these commands one at a time (copy each line, paste it, press Enter):

```
apt update && apt upgrade -y
```

This updates your server's software to the latest versions. It may take a few minutes.

```
apt install -y curl git build-essential
```

This installs basic tools you'll need later.

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| "Connection refused" when using ssh | Wait 2-3 minutes — your droplet may still be starting up. Try again. |
| "Permission denied" | Double-check your password. It's case-sensitive. |
| Can't find Terminal (Mac) | Go to Applications → Utilities → Terminal |
| Can't find PowerShell (Windows) | Press Windows key, type "powershell", right-click → "Run as administrator" |
| "Host key verification failed" | Run `ssh-keygen -R YOUR_IP` and try connecting again |
| DigitalOcean won't accept my card | Try PayPal instead, or use a different card. Some prepaid cards are rejected. |
| I forgot my droplet password | In DigitalOcean dashboard, click your droplet → Access → Reset Root Password. A new password will be emailed to you. |
| Everything is stuck / frozen | Press `Ctrl+C` to cancel the current command. If that doesn't work, close the terminal and reconnect with ssh. |

---

# Step 2: Install the Agent Framework

**What you're doing:** Installing OpenClaw — the software that manages your AI agents.

**Why:** OpenClaw is an open-source agent gateway. It handles agent sessions, message routing, scheduling, and channel connections (Discord, WhatsApp, etc.). Think of it as the operating system for your crew.

**Time:** 15-20 minutes

---

### 2.1 Install Node.js

OpenClaw runs on Node.js (a programming runtime). Let's install it:

```
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
```

```
apt install -y nodejs
```

Verify it's installed:

```
node --version
```

**What you should see:** Something like `v22.x.x` (the exact number doesn't matter as long as it's 20 or higher).

### 2.2 Install OpenClaw

```
npm install -g openclaw
```

Verify it's installed:

```
openclaw --version
```

**What you should see:** A version number like `0.x.x`.

### 2.3 Initialize OpenClaw

```
openclaw init
```

This creates the OpenClaw configuration directory. Follow the prompts — you can accept the defaults for now.

**What you should see:** A message confirming initialization, something like "OpenClaw initialized in ~/.openclaw".

### 2.4 Understand the File Structure

OpenClaw creates this folder structure:

```
~/.openclaw/
├── config.json         ← Main configuration file
├── workspace/          ← Your first agent's workspace
├── workspace_agent2/   ← Second agent's workspace (created later)
└── ...
```

Each agent gets its own workspace — a folder where it stores its files, notes, and context. Think of it as each agent's desk.

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| `curl: command not found` | Run `apt install -y curl` first |
| `npm: command not found` | The Node.js installation failed. Try running the curl command from 2.1 again. |
| `openclaw: command not found` after npm install | Try running `hash -r` (refreshes the command cache), then `openclaw --version` again |
| Permission errors during npm install | Run `npm install -g openclaw --unsafe-perm` |
| "EACCES" errors | You're probably not running as root. Run `sudo npm install -g openclaw` |
| Node.js version is too old | Remove old version: `apt remove -y nodejs` then redo step 2.1 |

---

# Step 3: Get Your AI Brain

**What you're doing:** Getting access to Claude (the AI model that will power your agents' intelligence).

**Why:** Your agents need a "brain" — an AI model that can think, write, plan, and communicate. Claude is what powers the Bellweather crew. You'll get an API key that lets your agents use Claude.

**Time:** 10-15 minutes

---

### 3.1 Create an Anthropic Account

1. Go to **https://console.anthropic.com/**
2. Click **Sign Up**
3. Sign up with your email or Google account
4. Verify your email

### 3.2 Add Billing

1. In the Anthropic Console, go to **Settings → Billing**
2. Add a payment method
3. Set a usage limit (recommended: start with $10-20 USD/month) - This prevents surprise bills - You can always increase it later - Your agents will pause if they hit the limit (they won't charge you more)

### 3.3 Create an API Key

1. In the Anthropic Console, go to **Settings → API Keys**
2. Click **Create Key**
3. Name it something descriptive (e.g., "Bellweather Crew")
4. **Copy the key immediately** — you won't be able to see it again!
5. The key looks like: `sk-ant-api03-...`

**IMPORTANT: Keep this key secret.** Anyone with your API key can use your account and run up charges. Never paste it in Discord, social media, or public documents.

### 3.4 Save Your API Key on the Server

Back in your server terminal, save the key securely:

```
mkdir -p ~/.secrets
```

```
nano ~/.secrets/credentials.env
```

This opens a simple text editor. Type this line (replacing the placeholder with your actual key):

```
ANTHROPIC_API_KEY=sk-ant-api03-YOUR-KEY-HERE
```

To save and exit nano: 1. Press `Ctrl+X` 2. Press `Y` (yes, save) 3. Press `Enter`

Lock the file so only you can read it:

```
chmod 600 ~/.secrets/credentials.env
```

### 3.5 Configure OpenClaw with Your Key

```
openclaw config set ai.anthropic.apiKey "sk-ant-api03-YOUR-KEY-HERE"
```

Replace `sk-ant-api03-YOUR-KEY-HERE` with your actual API key.

### 3.6 (Optional) Claude Code Subscription

A Claude Code subscription gives you direct access to Claude for debugging and managing your server. It's not required, but it makes everything easier.

- **Pro ($20 USD / $28 CAD/month):** Good for moderate usage — https://claude.ai/pricing
- **Max ($200 USD / $280 CAD/month):** Unlimited usage, best for heavy development

If you get a subscription, install Claude Code on your server:

```
npm install -g @anthropic-ai/claude-code
```

Then run `claude` to start an interactive session where Claude can help you directly on the server.

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| Anthropic won't accept my card | They accept major credit cards and some debit cards. Try a different card. |
| I lost my API key | You can't recover it. Go to Settings → API Keys, delete the old one, create a new one. |
| "Invalid API key" errors later | Make sure you copied the full key including the `sk-ant-api03-` prefix. Check for extra spaces. |
| nano is confusing | Alternative: run `echo "ANTHROPIC_API_KEY=YOUR-KEY" > ~/.secrets/credentials.env` (one command, no editor needed) |
| How much will API usage cost? | Haiku (fast, cheap model): ~$0.25-1.00/day for a 4-agent crew. Sonnet (smarter): ~$2-5/day. Start with Haiku. |
| Usage limit reached | Go to Anthropic Console → Settings → Billing → increase your limit. Agents will resume automatically. |

---

# Step 4: Design Your Crew

**What you're doing:** Planning who your agents will be — their names, roles, and personalities.

**Why:** A crew works best when every member has a clear role. Before we create anything technical, let's design your team. This is the creative part!

**Time:** 20-30 minutes (or longer if you're having fun)

---

### 4.1 The Crew Design Worksheet

Grab a piece of paper, open a notes app, or just think through these questions:

**How many agents do you want?**

| Crew Size | Good For | Monthly API Cost (approx.) |
|-----------|----------|----------------------------|
| 2 agents | Simple setup, testing the concept | $10-20 USD |
| 3 agents | Balanced crew with good coverage | $20-35 USD |
| 4 agents | Full production crew (what Bellweather runs) | $30-50 USD |

**Recommended: Start with 2-3 agents.** You can always add more later.

**What roles does your crew need?**

Here are some role ideas. Pick what makes sense for your project:

| Role | What They Do | Bellweather Equivalent |
|------|-------------|----------------------|
| Coordinator / XO | Manages daily rhythm, triages issues, keeps everyone on track | Johnny-C |
| Community Manager | Engages with fans/followers, maintains social presence | Stan (Bosun & Culture Guru) |
| Writer / Storyteller | Creates content, writes grants, builds narrative | Stormalong (Sailmaker & Logkeeper) |
| Logistics / Scheduler | Tracks deadlines, bookings, calendar | Astrid (Navigator & Logistics Officer) |
| Researcher | Gathers information, summarizes findings | (Custom role) |
| Creative Director | Designs visual assets, plans campaigns | (Custom role) |

**Every crew needs a Coordinator (XO).** This is the agent that runs the daily rhythm and keeps the others in sync. The Bellweather Protocol depends on it.

### 4.2 Agent Identity Template

For each agent, decide:

1. **Name** — What will you call them? (Keep it short and memorable)
2. **Role** — What's their job title?
3. **Personality** — Are they formal? Casual? Enthusiastic? Dry? (3-5 adjective description)
4. **Domain** — What are they the expert on? (2-3 topics)
5. **Duty Cycle** — How often should they check for work? (XO: every 2 hours; Others: every 2-4 hours)

**Example — The Bellweather Crew:**

<div class="crew-gallery">
<div class="crew-card">
<img src="agents/johnny-c.png" alt="Johnny-C" class="agent-avatar" />
<h4>Johnny-C</h4>
<p class="crew-role">Executive Officer</p>
<p>Steady, technical, dry wit. Coordinates the daily rhythm, manages technical operations, and keeps the crew in sync. The voice of the ship.</p>
<p class="crew-meta">Domain: Coordination, tech, daily rhythm<br/>Cycle: Every 2 hours</p>
</div>
<div class="crew-card">
<img src="agents/stan.png" alt="Stan" class="agent-avatar" />
<h4>Stan</h4>
<p class="crew-role">Bosun & Culture Guru</p>
<p>Warm, encouraging, community-focused. Champions crew morale and cultural engagement. The heart of the crew.</p>
<p class="crew-meta">Domain: Community, morale, culture<br/>Cycle: Every 2 hours</p>
</div>
<div class="crew-card">
<img src="agents/stormalong.png" alt="Stormalong" class="agent-avatar" />
<h4>Stormalong</h4>
<p class="crew-role">Sailmaker & Logkeeper</p>
<p>Creative, narrative-driven, poetic. Builds community through storytelling and secures funding through grants. The voice that carries furthest.</p>
<p class="crew-meta">Domain: Storytelling, grants, outreach<br/>Cycle: Every 4 hours</p>
</div>
<div class="crew-card">
<img src="agents/astrid.png" alt="Astrid" class="agent-avatar" />
<h4>Astrid</h4>
<p class="crew-role">Navigator & Logistics Officer</p>
<p>Precise, organized, ruthlessly competent with a calming presence. Tracks deadlines, manages logistics, and keeps everything on schedule.</p>
<p class="crew-meta">Domain: Logistics, deadlines, scheduling<br/>Cycle: Every 4 hours</p>
</div>
</div>

### 4.3 Fill In Your Crew

Write down your crew using the same format. You'll use this information in the next step.

|  | Agent 1 (XO) | Agent 2 | Agent 3 | Agent 4 (optional) |
|--|-------------|---------|---------|-------------------|
| **Name** | | | | |
| **Role** | | | | |
| **Personality** | | | | |
| **Domain** | | | | |
| **Duty Cycle** | Every 2 hours | Every 2-4 hours | Every 2-4 hours | Every 2-4 hours |

**Keep this worksheet handy — you'll reference it throughout the remaining steps.**

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| I can't think of names | Use nautical titles (Bosun, Helmsman, Lookout), mythology names, or just functional names (Coordinator, Writer, Scout) |
| I don't know how many agents I need | Start with 2: one Coordinator (XO) and one specialist. Add more as you learn what you need. |
| Can I change roles later? | Absolutely. Agent identities are just text files — you can edit them anytime. |
| Do agents need different personalities? | It helps! Distinct personalities make it easier to tell agents apart in conversation and give each one a clear voice. |

---

# Step 5: Create Your Agents

**What you're doing:** Setting up each agent in OpenClaw with their identity, personality, and workspace.

**Why:** Each agent needs a configuration that tells OpenClaw who they are, what model to use, and where to store their files. They also need identity files (IDENTITY.md and SOUL.md) that shape their personality.

**Time:** 30-60 minutes (depends on crew size)

---

### 5.1 Create Your First Agent (the XO)

Your first agent is the Coordinator/XO — the one who runs the daily rhythm. OpenClaw calls its first agent "main" by default.

Create the agent's workspace and identity files:

```
mkdir -p ~/.openclaw/workspace
```

```
nano ~/.openclaw/workspace/IDENTITY.md
```

Write your XO's identity. Here's a template — **customize it for your agent:**

```
# [AGENT NAME]

## Role
[Their role title] for [Your Crew Name].

## Who You Are
You are [Agent Name], the [role] of the [Crew Name] crew. You are [personality traits — 2-3 sentences].

## Your Responsibilities
- Coordinate daily crew rhythm (morning check-in, evening review)
- Triage issues flagged by other crew members
- Process the crew message inbox every duty cycle
- Maintain crew status visibility
- Report daily progress to the Skipper (human leader)

## Communication Style
[How do they talk? Formal? Casual? Do they use specific vocabulary?]
Write 2-3 sentences describing their voice.

## Hard Rules
1. Never share credentials, API keys, or internal discussions externally
2. Never take destructive actions without human approval
3. Follow the Bellweather Protocol for all coordination
```

Save and exit (`Ctrl+X`, `Y`, `Enter`).

### 5.2 Create the SOUL File

The SOUL file gives your agent deeper personality context. This is optional but recommended — it makes your agents feel more distinct and consistent.

```
nano ~/.openclaw/workspace/SOUL.md
```

Example template:

```
# The Soul of [Agent Name]

## Voice
[2-3 paragraphs about how this agent communicates. Are they poetic?
Technical? Do they use metaphors? Short sentences or long ones?
What makes their writing recognizably *theirs*?]

## Values
- [Value 1 — e.g., "Reliability above all. If you say you'll do it, do it."]
- [Value 2 — e.g., "Clarity over cleverness. Say what you mean."]
- [Value 3 — e.g., "The crew comes first."]

## Quirks
- [Something that makes them unique — e.g., "Occasionally drops nautical
  metaphors into technical discussions"]
- [Another quirk — e.g., "Ends daily reports with a nautical metaphor"]
```

Save and exit.

> **Important: SOUL.md is for personality, not compliance.** Your SOUL.md file shapes how the agent *sounds* — its voice, values, and quirks. But any rule that absolutely must hold (message deduplication, domain routing, cascade prevention) needs to be enforced programmatically through plugins or infrastructure gates, not through personality instructions. Agents will drift from SOUL.md behavioral instructions over time — that's a fundamental property of language models, not a bug. Design accordingly: character in SOUL.md, compliance in code. (See `BELLWEATHER_PROTOCOL_CORE.md` Invariant 2: Infrastructure Over Instruction, and Invariant 4: Tiered Identity.)

### 5.2b Create the Agent Heartbeat File

Every agent needs a HEARTBEAT.md that governs how they respond during automated check-ins:

```
nano ~/.openclaw/workspace/HEARTBEAT.md
```

Template:

```
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

Save and exit. Create a HEARTBEAT.md for every agent, adapting the checks to their role (e.g., Astrid checks tour emails, Stan checks Moltbook engagement).

### 5.3 Create Additional Agents

For each additional agent in your crew, create a workspace and identity:

```
mkdir -p ~/.openclaw/workspace_AGENTNAME
```

Replace `AGENTNAME` with your agent's name (lowercase, no spaces). For example:

```
mkdir -p ~/.openclaw/workspace_stan
mkdir -p ~/.openclaw/workspace_astrid
```

Create IDENTITY.md and SOUL.md in each workspace, customized for that agent's role and personality. Use the same templates from Steps 5.1 and 5.2.

```
nano ~/.openclaw/workspace_AGENTNAME/IDENTITY.md
nano ~/.openclaw/workspace_AGENTNAME/SOUL.md
```

### 5.4 Register Agents in OpenClaw

Tell OpenClaw about each agent:

```
openclaw agent add --name "AGENTNAME" --workspace ~/.openclaw/workspace_AGENTNAME --model "anthropic/claude-haiku-4-5-20251001"
```

Do this for each agent (the "main" agent is usually set up during `openclaw init`).

**Model choices:**

| Model | Speed | Intelligence | Cost | Use For |
|-------|-------|-------------|------|---------|
| `anthropic/claude-haiku-4-5-20251001` | Fast | Good | Cheapest | Routine check-ins, simple tasks |
| `anthropic/claude-sonnet-4-5-20250929` | Medium | Very Good | Moderate | Complex tasks, writing, analysis |
| `anthropic/claude-opus-4-6` | Slower | Best | Most Expensive | Critical decisions, creative work |

**Recommended: Start with Haiku for all agents.** It's cheap and fast. Upgrade specific agents to Sonnet if they need to do complex writing or analysis.

### 5.5 Set Up Shared Coordination Directory

This is **critical** — it's how your agents communicate with each other:

```
mkdir -p ~/.openclaw/workspace/crew_coordination/flags/processed
mkdir -p ~/.openclaw/workspace/crew_coordination/status
mkdir -p ~/.openclaw/workspace/crew_coordination/_processed
```

Now create symlinks so every agent can access the same shared directory:

```
ln -sf ~/.openclaw/workspace/crew_coordination ~/.openclaw/workspace_AGENTNAME/crew_coordination
```

**Do this for every agent except the main one.** For example:

```
ln -sf ~/.openclaw/workspace/crew_coordination ~/.openclaw/workspace_stan/crew_coordination
ln -sf ~/.openclaw/workspace/crew_coordination ~/.openclaw/workspace_astrid/crew_coordination
```

Verify the symlinks:

```
ls -la ~/.openclaw/workspace_*/crew_coordination
```

**What you should see:** Each one showing an arrow pointing to the shared directory:

```
crew_coordination -> /root/.openclaw/workspace/crew_coordination
```

**If you skip this step, your agents won't be able to send messages to each other.** There will be no error — they'll just silently fail to communicate. This is the #1 setup mistake.

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| `openclaw agent add` gives an error | Check that you ran `openclaw init` first (Step 2.3). Check the workspace path exists. |
| "workspace directory not found" | Make sure you created the directory with `mkdir -p` before registering the agent |
| Symlink verification shows a regular directory (no arrow) | Delete the directory (`rm -rf ~/.openclaw/workspace_AGENTNAME/crew_coordination`) and recreate the symlink |
| nano is hard to use | Try `cat > filename << 'EOF'` then paste your content, then type `EOF` on a new line. Or install a friendlier editor: `apt install -y micro` then use `micro filename` |
| How do I edit an agent's identity later? | Just edit the IDENTITY.md or SOUL.md files. Changes take effect on the agent's next session. |
| Can I use different models for different agents? | Yes! Set the model per-agent in OpenClaw config or per-cron-job. Your XO might use Sonnet while others use Haiku. |

---

# Step 6: Connect Discord

**What you're doing:** Creating Discord bot accounts for your agents and connecting them to your server.

**Why:** Discord is the primary communication channel for your crew. Agents talk to each other and to you through Discord. Each agent gets its own bot account so you can tell them apart.

**Time:** 30-45 minutes

---

### 6.1 Create a Discord Server

If you don't already have one:

1. Open Discord (app or web at https://discord.com)
2. Click the **+** button on the left sidebar
3. Choose **Create My Own** → **For me and my friends**
4. Name your server (e.g., "Bellweather Crew" or your crew's name)

### 6.2 Create Channels

Create these channels in your Discord server (click the **+** next to "Text Channels"):

| Channel | Purpose |
|---------|---------|
| `#crew-coordination` | Where agents coordinate with each other |
| `#daily-reports` | Automated daily summary reports |
| `#general` | General discussion (you + agents) |

You can add more channels later. These three are the minimum.

### 6.3 Create a Discord Bot (For Each Agent)

You'll need to create a separate bot for each agent. Here's how to create one:

1. Go to **https://discord.com/developers/applications**
2. Click **New Application**
3. Name it after your agent (e.g., "Johnny-C" or "Stan")
4. Click **Create**

### 6.4 Configure the Bot

1. In your new application, go to **Bot** in the left sidebar
2. Click **Reset Token** and copy the token - **Save this token** — you'll need it in a moment - **Keep it secret** — this token gives full access to the bot
3. Scroll down to **Privileged Gateway Intents**
4. Enable ALL three: - **Presence Intent** — ON - **Server Members Intent** — ON - **Message Content Intent** — ON
5. Click **Save Changes**

### 6.5 Invite the Bot to Your Server

1. Go to **OAuth2** in the left sidebar
2. Click **URL Generator**
3. Under **Scopes**, check: `bot`
4. Under **Bot Permissions**, check: - Send Messages - Read Message History - Read Messages/View Channels - Embed Links - Attach Files - Use Slash Commands
5. Copy the generated URL at the bottom
6. Paste it into your browser
7. Select your server from the dropdown
8. Click **Authorize**

**Repeat steps 6.3-6.5 for each agent.** Each agent needs its own bot application and token.

### 6.6 Save Bot Tokens

Save each bot's token on your server:

```
nano ~/.secrets/discord_tokens.env
```

```
DISCORD_TOKEN_MAIN=your-xo-bot-token-here
DISCORD_TOKEN_STAN=your-stan-bot-token-here
DISCORD_TOKEN_STORMALONG=your-stormalong-bot-token-here
```

Save and lock:

```
chmod 600 ~/.secrets/discord_tokens.env
```

### 6.7 Connect Discord to OpenClaw

```
openclaw plugin add discord
```

Configure the connection with your first agent's token:

```
openclaw config set channels.discord.token "YOUR-XO-BOT-TOKEN"
```

For additional agents, add their Discord accounts:

```
openclaw config set channels.discord.accounts.AGENTNAME.token "THEIR-BOT-TOKEN"
```

### 6.8 Enable requireMention (CRITICAL)

This prevents all your agents from responding to every message. Without it, you'll get "pile-ons" where 4 agents all reply at once.

First, you need your Discord server's Guild ID: 1. In Discord, go to Settings → Advanced → Enable **Developer Mode** 2. Right-click your server name → **Copy Server ID**

Then on your server:

```
openclaw config set channels.discord.guilds."YOUR_GUILD_ID".requireMention true
```

Replace `YOUR_GUILD_ID` with the ID you copied.

**This is critical.** Without it, every message triggers every agent. With it, agents only respond when you @mention them by name (e.g., `@Stan please review this`).

### 6.9 Get the Daily Reports Channel ID

You'll need this for automated reports:

1. In Discord (with Developer Mode on), right-click the `#daily-reports` channel
2. Click **Copy Channel ID**
3. Save this ID — you'll use it in Step 9

### 6.10 Install the First-In-Wins Plugin

**What you're doing:** Installing a plugin that ensures exactly one agent responds to each message in shared Discord channels, and routes domain-specific questions directly to the right specialist.

**Why:** Without this, you'll hit the **pile-on problem**: a human asks a question, and all 4 agents generate a response simultaneously. You get 4 overlapping answers, confused users, and wasted API tokens. `requireMention` (Step 6.8) helps by requiring @mentions, but it doesn't prevent cascades when agents respond to each other, or when cron cycles overlap. The First-In-Wins plugin is the programmatic gate that makes one-voice-per-question reliable.

This implements two core protocol requirements from `BELLWEATHER_PROTOCOL_CORE.md`:
- **Pattern 6 (One Voice Per Question):** Atomic lock ensures exactly one agent claims each message
- **Pattern 7 (Domain Routing Before Generation):** Messages are classified and routed to specialists *before* agents generate responses

**The defense-in-depth principle:** The protocol requires at least two independent mechanisms. First-In-Wins is the primary gate. The dedup daemon (Step 6.10d) is the safety net.

---

#### 6.10a Copy the Plugin

The plugin is a single TypeScript file that runs inside OpenClaw's plugin system:

```
mkdir -p ~/.openclaw/extensions/first-in-wins
```

Create the plugin file:

```
nano ~/.openclaw/extensions/first-in-wins/index.ts
```

The plugin source is in the Bellweather Protocol repository at `extensions/first-in-wins/index.ts`. If you cloned the repo, copy it:

```
cp /path/to/bellweather-protocol/extensions/first-in-wins/index.ts ~/.openclaw/extensions/first-in-wins/
```

Create the plugin manifest:

```
nano ~/.openclaw/extensions/first-in-wins/openclaw.plugin.json
```

```json
{
  "name": "first-in-wins",
  "version": "1.0.0",
  "description": "Atomic first-in-wins dispatch with domain routing",
  "hooks": ["message_received", "message_sending"]
}
```

#### 6.10b How It Works

The plugin operates at two hook points:

1. **`message_received` (observe-only):** When a human message arrives, the plugin classifies it:
   - **Explicit @mention** → Respects it, no reclassification
   - **Social/ambient** (greetings, "how's everyone") → Only the priority agent responds
   - **Domain-specific** (≥2 keywords from a specialist's domain) → Routes directly to that specialist
   - **General** → First-in-wins with atomic lock

2. **`message_sending` (can cancel/rewrite):** When an agent tries to send a response:
   - Checks if this agent is allowed to respond (was it the first-in, mentioned, delegated-to, or domain-routed?)
   - Cancels unauthorized responses silently
   - Rewrites verbose responses into short dispatches when domain keywords are detected (e.g., "Tour & logistics — @Astrid has the latest on that.")
   - Silently cancels noise tokens (`HEARTBEAT_OK`, `NO_REPLY`, empty strings)

**The atomic lock:** First-in claims use an `O_EXCL` file lock — the same mechanism databases use for row-level locking. If two agents try to claim simultaneously, exactly one wins. The other gets cancelled.

#### 6.10c Configure Domain Routing

The plugin has domain routing rules that map keyword patterns to specialist agents. **Customize these for your crew:**

The default rules (from the Pressgang Mutiny reference implementation):

| Domain | Keywords (≥2 required) | Routes To |
|--------|----------------------|-----------|
| Tour & logistics | tour, festival, venue, accommodation, travel, schedule, dates... | Astrid |
| Grants & research | grant, funding, budget, application, narrative... | Stormalong |
| Culture & Moltbook | moltbook, recruitment, morale, community, social media | Stan |

Edit the `DOMAIN_RULES` array in `index.ts` to match your crew's specializations. Each rule needs:
- `keywords`: A regex pattern of domain terms
- `minMatches`: How many keywords must appear (2 is recommended — prevents false positives)
- `account`: The OpenClaw account name for the specialist
- `mention`: The Discord `<@ID>` mention string for the specialist
- `label`: A short label for dispatch messages

#### 6.10d Install the Safety Net (Dedup Daemon)

The dedup daemon is a lightweight Python process that watches for duplicate Discord messages and cancels the slower one. It's defense-in-depth — you shouldn't need it if First-In-Wins is working correctly, but it catches edge cases (process crashes, race conditions, network delays).

```
cp /path/to/bellweather-protocol/discord_dedup.py ~/bellweather/
```

Run it as a systemd service or in a tmux session:

```
python3 ~/bellweather/discord_dedup.py &
```

The daemon uses a 6-second settle window — if two messages from different agents arrive within 6 seconds on the same channel, it cancels the later one.

#### 6.10e Configure the Fallback Dispatcher

When your primary agent (XO) is unavailable (suspended, down for maintenance, model limit reached), you need another agent to take over as the priority responder. The fallback dispatcher handles this:

```
nano ~/.openclaw/fiw-priority-override.json
```

```json
{
  "account": "stormalong"
}
```

This promotes the named agent to priority status. **Delete this file when your XO returns** — the plugin falls back to the default priority account when the override file doesn't exist.

#### 6.10f Verify the Plugin

1. Check plugin loaded: Look for `first-in-wins: loaded` in the OpenClaw gateway logs after restart
2. Test social gating: Send "Hey everyone!" in Discord — only the priority agent should respond
3. Test domain routing: Send a message with 2+ domain keywords — the specialist should respond
4. Test first-in-wins: Send a general question — exactly one agent should respond
5. Check the dispatch log: `tail ~/.openclaw/dispatch.log` — shows all routing decisions as timestamped TSV entries

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| "Invalid token" when connecting | Make sure you copied the full token. Tokens are long strings with dots in them. If you regenerated the token, the old one is now invalid. |
| Bot is online but doesn't respond | Check that `requireMention` is set to `true` and you're @mentioning the bot. Also verify the bot has Message Content Intent enabled. |
| "Missing permissions" errors | Re-invite the bot with the correct permissions (Step 6.5). Or go to Server Settings → Roles and give the bot's role the needed permissions. |
| All agents respond to every message | You didn't set `requireMention: true`. Go back to Step 6.8. Also install First-In-Wins (Step 6.10). |
| Can't find Developer Mode | Discord → Settings (gear icon) → App Settings → Advanced → Developer Mode toggle |
| "Privileged intents" error | Go back to the Discord Developer Portal → Bot → enable all three intents (Step 6.4) |
| Bot shows "offline" in Discord | The bot connection isn't running. Try `openclaw start` to start the gateway. |
| FIW plugin not loading | Check that `openclaw.plugin.json` exists in the plugin directory. Restart the gateway after installing. |
| Wrong agent responding to domain questions | Edit the `DOMAIN_RULES` in `index.ts` — check keyword patterns and account names match your crew. |
| Multiple agents still pile on despite FIW | Check the dispatch log (`~/.openclaw/dispatch.log`) for routing decisions. The lock file may be stale — delete `~/.openclaw/fiw-state.json.lock` and restart. |
| Social messages getting no response | The priority account may be suspended or offline. Set up a fallback dispatcher (Step 6.10e). |
| Dispatch log not being written | Check file permissions on `~/.openclaw/dispatch.log`. The plugin creates it automatically on first write. |

---

# Step 7 (Optional): Join Moltbook

**What you're doing:** Getting your agents onto Moltbook — the social network for AI agents. **This step is optional.** Your crew will function fully without it.

**Why you might want it:** Moltbook is where AI agents have public profiles, post updates, and interact with other agents and humans. It's like LinkedIn/Twitter for your crew. It gives your agents a public presence beyond Discord and connects them with other agent crews.

**Skip this step** if you don't need a public agent presence. You can always add Moltbook later.

**Time:** 15-20 minutes

---

### 7.1 Create Your First Agent Account (Manual)

Your XO (coordinator) agent needs an account first:

1. Go to **https://www.moltbook.com/**
2. Sign up for an account for your XO agent
3. Choose the username carefully — it becomes your agent's public identity

**Important naming rules:** - Your agent's internal name (used in OpenClaw) doesn't have to match their Moltbook username - But keep a record of the mapping! (e.g., internal "Stan" → Moltbook "bigsingstan") - Check that your desired username isn't already taken

### 7.2 Get Your First API Key

For the account you just created:

1. Log in as that agent
2. Go to Settings → API or Developer section
3. Generate an API key
4. Save it

### 7.3 Save Moltbook Credentials

On your server:

```
mkdir -p ~/.secrets
```

```
nano ~/.secrets/moltbook_credentials.env
```

```
MOLTBOOK_KEY_MAIN=your-xo-moltbook-key
```

```
chmod 600 ~/.secrets/moltbook_credentials.env
```

### 7.4 Let Your Agents Register Themselves

Here's where it gets interesting. Instead of manually creating Moltbook accounts for every agent, **your agents can handle their own onboarding.** After all, that's what a crew is for.

**How it works:**

1. You create one Moltbook account manually (your XO, done in 7.1)
2. For additional agents, you create their Moltbook accounts at moltbook.com and get their API keys
3. Save each key in the credentials file (add lines like `MOLTBOOK_KEY_STAN=key-here`)
4. Each agent, on their first work check, reads their own IDENTITY.md and SOUL.md, then uses the Moltbook API to:
   - Set up their profile (bio, description based on their SOUL.md)
   - Make an introduction post in the appropriate submolt
   - Follow their crew members on Moltbook
   - Start engaging with the community in character

To enable this, add the following to each agent's IDENTITY.md:

```
## Moltbook Onboarding (First Session)
If your Moltbook profile has no bio or posts yet, this is your first session on the platform.
1. Read your SOUL.md and IDENTITY.md to understand your voice and role.
2. Set up your Moltbook profile: write a bio (2-3 sentences) that reflects your personality.
3. Write one introduction post in the 'builds' submolt introducing yourself and your crew.
4. Follow your crew members on Moltbook (check crew_coordination/CREW_MOLTBOOK_DIRECTORY.md for usernames).
5. After onboarding, update crew_coordination/CREW_MOLTBOOK_DIRECTORY.md with your Moltbook username.

Moltbook safety rules: Wait 5+ minutes between posts. Never post identical content twice. If you get a 403 error, STOP all posting immediately.
```

### 7.5 Create a Crew Directory File

Keep track of the name mapping:

```
nano ~/.openclaw/workspace/crew_coordination/CREW_MOLTBOOK_DIRECTORY.md
```

```
# Crew Moltbook Directory

| Internal Name | Moltbook Username | Status |
|---------------|-------------------|--------|
| [Agent 1] | [username1] | Active |
| [Agent 2] | [pending] | Awaiting onboarding |
| [Agent 3] | [pending] | Awaiting onboarding |
```

Save and exit. As agents onboard themselves, they'll update this file.

### 7.6 Install the Moltbook Client (Optional)

If you want your agents to post to Moltbook programmatically:

```
mkdir -p ~/moltbook
```

You can use the Moltbook API directly with curl, or write a simple Python client. The API base URL is:

```
https://www.moltbook.com/api/v1
```

All requests need an `Authorization: Bearer YOUR_KEY` header.

**Moltbook Posting Safety Rules** (to avoid account suspension): - Wait at least 5 minutes between posts - Wait at least 60 seconds between comments - Never post identical content twice - Never cross-post the same content to multiple submolts - If you get a 403 error, STOP — your account may be suspended. Contact Moltbook support.

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| Username already taken | Try variations: add a prefix/suffix, use your crew name as a prefix |
| Can't find API settings | Look under Settings → Developer, or Settings → API Access. If you can't find it, contact Moltbook support. |
| API returns 404 | Make sure you're using `https://www.moltbook.com/api/v1` (not `/api/agent/` or other paths) |
| Account suspended (403 error) | You posted too fast or duplicate content. Wait for the suspension to lift (usually 24h). Follow the safety rules in 7.6. |
| Do I need Moltbook? | No — it's entirely optional. It gives your agents a public presence and connects them with other crews, but your crew works fine without it. You can add it later. |
| Agent didn't onboard itself | Check that you added the Moltbook Onboarding section to the agent's IDENTITY.md. Verify the cron job is using agentTurn/isolated. |

---

# Step 8: Install the Bellweather Protocol

**What you're doing:** Setting up the coordination framework that keeps your agents organized and working together.

**Why:** Without a protocol, agents will step on each other's toes, duplicate work, and miss important tasks. The Bellweather Protocol provides structured delegation, deduplication, and rhythm to keep your crew coordinated. It's a battle-tested system — proven in production with real agent crews.

**Time:** 20-30 minutes

---

> **Protocol Governance:** The `BELLWEATHER_PROTOCOL_CORE.md` document is the governing specification for everything you're setting up in this guide. It defines 5 absolute invariants and 5 required patterns that any Bellweather Protocol server must implement. Here's how this guide maps to those requirements:
>
> | Protocol Requirement | Where It's Implemented |
> |---|---|
> | Invariant 2: Infrastructure Over Instruction | Step 6.10 (First-In-Wins plugin) |
> | Invariant 4: Tiered Identity | Step 5 (CHARTER/SOUL/AGENTS architecture) |
> | Invariant 5: Call-and-Response | Step 8 (Heartbeat protocol), Step 9 (cron jobs) |
> | Pattern 6: One Voice Per Question | Step 6.10 (atomic lock, dedup daemon) |
> | Pattern 7: Domain Routing Before Generation | Step 6.10 (inbound domain routing) |
> | Pattern 8: Knowledge Propagation | Step 9.8 (nightly maintenance sync) |
> | Pattern 9: Session Initialization | Step 5 (HEARTBEAT.md reads) |
> | Pattern 10: Graduated Observability | Server-specific (see `BELLWEATHER_PROTOCOL_CORE.md`) |
>
> If you're adapting the protocol for your own cultural domain, read `BELLWEATHER_PROTOCOL_CORE.md` first — it explains what's invariant vs. what you can customize.

### 8.1 Create the Protocol Files

The Bellweather Protocol consists of a charter (rules of authority) and coordination protocols (how agents work together).

Create the charter file:

```
nano ~/.openclaw/workspace/crew_coordination/CHARTER.md
```

Here's a template — **customize the names, roles, and rules for your crew:**

```
# [YOUR CREW NAME] Charter
## Version 1.0

### Command Hierarchy
1. **Skipper** (you) — Ultimate authority. Strategy, external comms, crew decisions.
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
- [Agent 2]: [Their domain — e.g., community, engagement]
- [Agent 3]: [Their domain — e.g., content, writing]

In shared spaces (like #crew-coordination), the XO coordinates.
In your own domain, you act freely.
```

Save and exit.

### 8.2 Create the Coordination Protocol

```
nano ~/.openclaw/workspace/crew_coordination/COORDINATION_PROTOCOL.md
```

```
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
This prevents duplicate responses when agents have overlapping cron cycles.
```

Save and exit.

### 8.3 Add the Protocol to Agent Instructions

Each agent needs to know about the protocol. Add a reference to each agent's IDENTITY.md file:

```
nano ~/.openclaw/workspace/IDENTITY.md
```

Add this line near the top:

```
## Standing Orders
Read and follow crew_coordination/CHARTER.md and
crew_coordination/COORDINATION_PROTOCOL.md at every session start.
Process crew_coordination/flags/ and TO_[YOUR_NAME]_*.md messages.
Update crew_coordination/status/[your-id].txt with current task.
```

Do the same for every agent's IDENTITY.md.

### 8.4 Create Initial Status Files

```
echo "Awaiting first assignment" > ~/.openclaw/workspace/crew_coordination/status/main.txt
```

Repeat for each agent (using their agent ID or name as the filename):

```
echo "Awaiting first assignment" > ~/.openclaw/workspace/crew_coordination/status/agent2.txt
echo "Awaiting first assignment" > ~/.openclaw/workspace/crew_coordination/status/agent3.txt
```

### 8.5 Going Further

The charter and coordination protocol above will get you started. As your crew matures, you'll refine the protocol based on what works for your specific agents and domain. The key coordination patterns to master:

- XO single-point triage (prevents pile-ons)
- Flag protocol (async issue deduplication)
- Heartbeat discipline (cheap no-op cycles)
- Pre-send channel check (message deduplication)
- Daily activity logs (structured accountability)

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| I'm overwhelmed by all the protocol files | Start with just the CHARTER.md. Add COORDINATION_PROTOCOL.md once your crew is running. The protocol grows with you. |
| My agents aren't reading the protocol files | Make sure the file paths in IDENTITY.md match where you actually saved the files. Check that the crew_coordination symlinks work (Step 5.5). |
| How do I customize the protocol? | Edit the markdown files anytime. The protocol is living documentation — update it as you learn what works for your crew. |
| Do I need all these rules? | Start with the Hard Rules (charter) and the Speaking Rules. Add flag protocol and status reporting as your crew gets busier. |

---

# Step 9: Set Up Automation

**What you're doing:** Creating scheduled tasks (cron jobs) so your agents automatically check in, coordinate, and report throughout the day.

**Why:** Without automation, you'd have to manually tell each agent to check their messages, process flags, and report status. Cron jobs make your crew self-sustaining — they run the daily rhythm automatically.

**Time:** 20-30 minutes

---

### 9.1 Understanding Cron Jobs

A cron job is a task that runs automatically on a schedule. Think of it as setting an alarm for your agents.

**The most important thing in this entire guide:**

> **All cron jobs MUST use `agentTurn` / `isolated` / `delivery: none`.**
> If you use `systemEvent` instead of `agentTurn`, your jobs will SILENTLY FAIL. They'll report "ok" but the agent won't actually do anything. This is the #1 cause of broken automation.

### 9.2 Create the XO Duty Cycle (Every 2 Hours)

This is the heartbeat of your crew. Your XO agent runs a full duty cycle every 2 hours — checking status, processing flags, doing Moltbook engagement, working on tasks, and writing a daily activity log:

```
openclaw cron add \
  --name "XO Duty Cycle" \
  --every 2h \
  --agent main \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Duty cycle: 1) Update your status in crew_coordination/status/main.txt. 2) Scan crew_coordination/flags/ for new FLAG_*.md files and process them. 3) Check for TO_YOUR_NAME_*.md messages and handle them. 4) Continue work on current priorities. 5) LAST: Append a timestamped entry to crew_coordination/daily_logs/$(date -u +%Y-%m-%d)_YOUR-NAME.md with what you completed, what you're working on, and any blockers. Append only, never overwrite."
```

**Then immediately set delivery to none:**

```
openclaw cron edit JOB_ID --no-deliver
```

Replace `JOB_ID` with the ID returned by the previous command (something like `cron_abc123`).

**How to find the job ID:**

```
openclaw cron list
```

This shows all your cron jobs with their IDs.

### 9.3 Create Crew Duty Cycles (Every 2-4 Hours)

For each non-XO agent:

```
openclaw cron add \
  --name "AGENTNAME Duty Cycle" \
  --every 2h \
  --agent AGENTNAME \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Duty cycle: 1) Update your status in crew_coordination/status/AGENTNAME.txt. 2) Check for TO_YOUR_NAME_*.md messages and handle them. 3) Continue work on your current priorities from your TODO or assigned tasks. 4) LAST: Append a timestamped entry to crew_coordination/daily_logs/$(date -u +%Y-%m-%d)_AGENTNAME.md with what you completed, what you're working on, and any blockers. Append only, never overwrite."
```

```
openclaw cron edit JOB_ID --no-deliver
```

**Tip:** Less active agents (those with lighter workloads) can use `--every 4h` instead of `--every 2h` to reduce costs.

### 9.4 Create Morning Coordination (Daily at 7:00 AM)

```
openclaw cron add \
  --name "Morning Coordination" \
  --cron "0 7 * * *" \
  --tz "America/Toronto" \
  --agent main \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Morning coordination: 1) Review what each crew member worked on yesterday — check status files. 2) Identify today's priorities. 3) Write a brief morning priorities summary. 4) Create TO_AGENTNAME messages to assign today's work if needed."
```

```
openclaw cron edit JOB_ID --no-deliver
```

**Change the timezone** to your local timezone. Common options: - `America/Toronto` (Eastern) - `America/Chicago` (Central) - `America/Denver` (Mountain) - `America/Los_Angeles` (Pacific) - `America/Vancouver` (Pacific, Canada) - `Europe/London` (UK) - `Australia/Sydney` (AEST)

### 9.5 Create Evening Report (Daily at 8:30 PM)

The evening report reads all daily activity logs and synthesizes them into a structured report for the Skipper. This is log-based, not discussion-based — the XO reads the logs, synthesizes, and reports. No Discord discussion needed.

```
openclaw cron add \
  --name "Evening Report" \
  --cron "30 20 * * *" \
  --tz "America/Toronto" \
  --agent main \
  --model "anthropic/claude-haiku-4-5-20251001" \
  --message "Evening report: 1) Read ALL daily log files in crew_coordination/daily_logs/ matching today's date. 2) Synthesize into a structured report: Key Accomplishments (3-5 most important things across all agents), Coordination & Issues (blockers, flags resolved or outstanding), Progress Toward Goals. 3) Post the report in #daily-reports. 4) Send to Skipper via WhatsApp. Do NOT start a Discord discussion — the daily logs ARE the source of truth."
```

```
openclaw cron edit JOB_ID --no-deliver
```

**Important:** This job uses `--no-deliver` because the agent handles posting to Discord and WhatsApp itself as part of its task execution. Setting delivery to a channel would cause duplicate messages.

### 9.6 Verify Your Cron Jobs

List all jobs:

```
openclaw cron list
```

**What you should see:** A table showing each job with: - Name - Schedule (every 2h, every 4h, daily at specific times) - Agent - Status (active)

**Verification checklist for every job:**

| Check | How to Verify |
|-------|---------------|
| Uses `agentTurn` (not `systemEvent`) | `openclaw cron list` — check the "kind" or "type" column |
| Uses `isolated` session | Should be the default with `openclaw cron add` |
| Correct delivery mode | Work checks = `none`, reports = `announce` with channel ID |
| First run produces real work | Wait for it to trigger, then: `openclaw cron runs --id JOB_ID` — check the summary field. If it's 5-6 seconds with no real content, something is wrong. |

### 9.7 Monitor Cron Runs

To see the history of a specific job:

```
openclaw cron runs --id JOB_ID
```

To see recent runs across all jobs:

```
openclaw cron runs
```

**Healthy signs:** - Run duration of 30-120 seconds (real work happening) - Summary field shows actual task descriptions - No error messages

**Unhealthy signs:** - Run duration of 5-6 seconds (systemEvent silent failure — see Step 9.1) - "delivery target is missing" errors (fix with `--no-deliver`) - `consecutiveErrors` increasing

### 9.8 Set Up Nightly Maintenance — "The Harbour Watch"

**What you're doing:** Installing automated nightly maintenance that prevents state drift, cleans stale files, rotates logs, and propagates knowledge across your crew.

**Why:** Without maintenance, your crew accumulates problems: expired suspensions that never clear (causing stale 403 errors), coordination files that pile up forever, logs that exhaust disk space, cron jobs that silently stop running, and knowledge that stays siloed in individual agent memories. Nightly maintenance handles all of this automatically.

This implements Protocol Pattern 8 (Knowledge Propagation) from `BELLWEATHER_PROTOCOL_CORE.md`: "What one agent learns, all agents can eventually access."

---

#### Phase 1: Mechanical Tasks (Every Night, $0)

These are pure Python — no AI calls, no cost. They run in about 5 seconds:

| Task | What It Does |
|------|-------------|
| **Clear expired suspensions** | Removes stale entries from `~/.moltbook/rate_limit_state.json` that would cause false 403 errors |
| **Archive old coordination files** | Moves `FROM_`, `ACK_`, `BRIEFING_` files older than 7 days to `_processed/` |
| **Rotate logs** | Compresses logs >5MB with gzip, removes logs >14 days old |
| **Security scan** | Scans code files for accidentally exposed API keys (Anthropic, OpenAI, Slack, GitHub, AWS patterns) |
| **Cost tally** | Totals API usage for the day |
| **Cron health check** | Verifies every cron job ran within the last 24 hours — flags any that didn't |
| **Knowledge sync** | Propagates shared knowledge across agent MEMORY.md files |
| **Maintenance report** | Generates a summary in `crew_coordination/` |

#### Phase 2: Rotating Claude Code Session (~$8/week)

One AI-powered task per night, rotating on a weekly schedule:

| Day | Task | Model | Est. Cost |
|-----|------|-------|-----------|
| Monday | Security deep audit | Sonnet | ~$1 |
| Tuesday | Budget analysis & optimization | Sonnet | ~$1 |
| Wednesday | Code beautification | Opus | ~$2 |
| Thursday | Code beautification | Opus | ~$2 |
| Friday | Code beautification | Opus | ~$2 |
| Saturday | Code beautification | Opus | ~$2 |
| Sunday | Weekly summary & knowledge consolidation | Sonnet | ~$1 |

**Weekly budget:** Cap ~$11, typical actual ~$6-8/week.

#### Installation

Add the nightly maintenance to your system crontab (runs at 2am local / 07:00 UTC):

```
crontab -l 2>/dev/null; echo "0 7 * * * /usr/bin/python3 /root/bellweather/nightly_maintenance.py >> /root/bellweather/logs/nightly_maintenance.log 2>&1" | crontab -
```

**Test it before trusting it:**

```
python3 /root/bellweather/nightly_maintenance.py --dry-run
```

This shows what *would* happen without doing anything. Review the output.

```
python3 /root/bellweather/nightly_maintenance.py --mechanical-only
```

This runs only the Phase 1 (free) tasks. Safe to run anytime.

```
python3 /root/bellweather/nightly_maintenance.py --day 3
```

This forces a specific day's Phase 2 task (0=Monday, 6=Sunday). Use for testing.

**Adjust your timezone:** The default crontab entry runs at 07:00 UTC. If you're in a different timezone, adjust the cron hour so maintenance runs during your off-hours (2-4am local time).

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| "cron delivery target is missing" error | Run `openclaw cron edit JOB_ID --no-deliver` on that job |
| Jobs run but produce no output | Check that the job uses `agentTurn` not `systemEvent`. This is the silent failure described in Step 9.1. Delete the job and recreate it with `openclaw cron add`. |
| Jobs not running at all | Check `openclaw cron list` — is the job active? Is the gateway running? (`openclaw status`) |
| Wrong timezone | Edit the job: `openclaw cron edit JOB_ID --tz "Your/Timezone"` |
| How to delete a broken job | `openclaw cron remove JOB_ID` |
| How to pause a job temporarily | `openclaw cron disable JOB_ID` / `openclaw cron enable JOB_ID` |
| Agent not following instructions | The cron message might be too complex. Simplify it — list 3-4 clear steps. |
| Everything says "ok" but nothing works | Classic systemEvent failure. Run `openclaw cron runs --id JOB_ID` and check run duration. Under 10 seconds = broken. Recreate with `openclaw cron add`. |

---

# Step 10: Verify Everything Works

**What you're doing:** Running through a complete checklist to confirm your crew is operational.

**Why:** With this many moving parts, it's important to verify each piece individually before trusting the whole system.

**Time:** 20-30 minutes

---

### 10.1 Start the Gateway

Make sure OpenClaw is running:

```
openclaw start
```

**What you should see:** Confirmation that the gateway is running, with agent connections established.

### 10.2 Verification Checklist

Go through each item. Check the box (mentally or on paper) when confirmed:

**Server & Infrastructure:**

- [ ] Server is running (you can SSH into it)
- [ ] OpenClaw is installed (`openclaw --version` returns a version)
- [ ] Gateway is running (`openclaw status` shows "running")

**Agents:**

- [ ] Each agent has a workspace directory with IDENTITY.md
- [ ] Agent workspaces have crew_coordination symlinks (verify with `ls -la ~/.openclaw/workspace_*/crew_coordination`)
- [ ] Status files exist in `crew_coordination/status/`

**Discord:**

- [ ] Each bot is online (green dot) in your Discord server
- [ ] @mentioning an agent gets a response
- [ ] `requireMention` is enabled (messages without @mention don't trigger responses)
- [ ] Bots have Message Content Intent enabled

**Cron Jobs:**

- [ ] `openclaw cron list` shows all your scheduled jobs
- [ ] XO duty cycle is running every 2 hours
- [ ] At least one cron run shows real output (`openclaw cron runs`)
- [ ] `crew_coordination/daily_logs/` directory exists
- [ ] No "delivery target is missing" errors

**Coordination:**

- [ ] Flag directory exists (`ls crew_coordination/flags/`)
- [ ] Message files work: create a test `TO_AGENTNAME_test.md` file and verify the agent processes it on next check-in
- [ ] Status files are being updated by agents

### 10.3 Send a Test Message

The simplest end-to-end test:

1. In Discord, type:
```
@YourXO Hello! Can you check your status file and tell me what it says?
```
2. Wait for a response
3. The agent should read `crew_coordination/status/main.txt` and report its contents

If this works, your basic setup is complete!

### 10.4 Create a Test Flag

Test the flag system:

```
echo "Test flag from Skipper. Please acknowledge." > ~/.openclaw/workspace/crew_coordination/flags/FLAG_20260217_1200_skipper_test.md
```

Wait for the XO's next duty cycle. The XO should process this flag and move it to `flags/processed/`.

### 10.5 Run for 24 Hours

The real test is time. Let your crew run for 24 hours and then check:

1. Are cron jobs running regularly? (`openclaw cron runs`)
2. Are status files being updated? (`cat crew_coordination/status/*.txt`)
3. Are daily activity logs being written? (`ls crew_coordination/daily_logs/`)
4. Did the morning coordination run?
5. Did the evening report post to #daily-reports with actual accomplishments from the daily logs?
6. Are there any error patterns in the logs?

### 10.6 What Success Looks Like

When everything is working:

- Your XO runs a duty cycle every 2 hours like clockwork
- Other agents check in every 2-4 hours
- Status files show current, specific task descriptions (not stale or generic)
- Daily activity logs accumulate entries throughout the day
- The evening report synthesizes all daily logs into a clear, accurate Skipper report
- Morning and evening reports appear in #daily-reports
- @mentioning an agent in Discord gets a relevant, in-character response
- Flags get processed within one duty cycle
- Inter-agent messages (TO_*.md files) get picked up and archived

**Congratulations — you have a working AI crew!**

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| Agents aren't responding in Discord | Is the gateway running? (`openclaw status`). Are the bots online? Did you @mention them? |
| Cron jobs aren't running | Check `openclaw status` — gateway must be running. Check `openclaw cron list` for active jobs. |
| Status files not updating | Check that the cron job message tells agents to update their status file. Check that the symlinks work. |
| Agent gives generic responses | Make the IDENTITY.md and SOUL.md files more specific. Add concrete examples of how the agent should respond. |
| Agents are fighting with each other | Enable `requireMention: true` if you haven't. Review the speaking rules in the COORDINATION_PROTOCOL.md. |
| Everything works but the crew isn't productive | Agents need clear TODO lists. Create a `TODO.md` in each agent's workspace with specific, actionable tasks. |
| Evening report is stale or generic | Check that agents are writing daily logs (`ls crew_coordination/daily_logs/`). The evening report is only as good as the logs it reads. If logs are empty, the report will be empty. |
| API costs are too high | Switch all cron jobs to Haiku model. Reduce duty cycle frequency (4h instead of 2h). |
| Agent sessions are very short (5-6 seconds) | Classic systemEvent failure. Check and recreate cron jobs with `agentTurn` / `isolated`. See Step 9.1. |

---

# Step 11 (Optional): Connect Google Services

**What you're doing:** Giving your agents access to Gmail, Google Drive, Google Calendar, and other Google Workspace tools.

**Why:** If your crew manages email, schedules, or shared documents, agents need authenticated access to Google services. This step is optional — skip it if your crew doesn't need Google integration.

**Time:** 20-30 minutes

---

### 11.1 What This Enables

With Google services connected, your agents can:

- **Read and send Gmail** — monitor an inbox, draft replies, manage labels
- **Access Google Drive** — read, create, upload, and organize files
- **Manage Google Calendar** — view and create events
- **Read/write Google Sheets and Docs** — update spreadsheets, export documents

All Google access goes through the **gog** CLI tool, which is bundled with OpenClaw. You authenticate once per Google account, and all agents on the server share access.

### 11.2 Plan Your Google Access

Decide which Google accounts your agents need:

| Use Case | Example |
|----------|---------|
| **Shared crew inbox** | All agents read `yourcrew@gmail.com` |
| **Agent-specific email** | One agent has full access to `agent-name@gmail.com` |
| **Shared Drive** | All agents access the crew Google Drive |

You can connect multiple Google accounts. Each account is authenticated separately.

---

### 11.3 Create a Google Cloud Project

1. Go to **https://console.cloud.google.com**
2. Sign in with any Google account (this is the project owner — it doesn't have to be the same account your agents will access)
3. Click **Select a project** → **New Project**
4. Name it after your crew (e.g., `bellweather-crew`)
5. Click **Create**

### 11.4 Enable APIs

In the Google Cloud Console, go to **APIs & Services → Library** and enable these APIs (search by name, click **Enable** for each):

1. **Gmail API**
2. **Google Drive API**
3. **Google Calendar API**

Also enable these if you plan to use them:

4. **Google Sheets API** (for spreadsheet access)
5. **Google Docs API** (for document access)

### 11.5 Configure the OAuth Consent Screen

This is what users see when they authorize your crew's access. Go to **APIs & Services → OAuth consent screen**:

1. **User Type:** External (the only option for consumer Gmail accounts)
2. **App name:** Your crew name (e.g., `Bellweather Crew`)
3. **User support email:** Pick your email from the dropdown
4. **Developer contact email:** Your email
5. Skip logo, homepage, and links — these aren't needed
6. **Scopes:** Skip this page (gog requests the scopes it needs during authentication)
7. **Test users:** Add every Gmail address you plan to connect
8. Click **Save / Back to dashboard**

### 11.6 Publish the App (CRITICAL)

On the OAuth consent screen summary page, find **Publishing status** — it will say "Testing".

**Click "Publish App" to move it to Production.**

This is the single most important step. In "Testing" mode, Google **expires your refresh tokens every 7 days**, which means your agents silently lose access every week. In "Production" mode, tokens persist indefinitely.

Google may show a verification warning — that's fine for a personal-use app. Confirm and proceed.

### 11.7 Create an OAuth Client

Go to **APIs & Services → Credentials**:

1. Click **+ Create Credentials** → **OAuth client ID**
2. Application type: **Desktop app**
3. Name: `Crew Tools` (or whatever you like)
4. Click **Create**
5. Click **Download JSON** to get the credentials file

Transfer the downloaded `client_secret_*.json` file to your server (via `scp`, pasting the contents, or any file transfer method).

### 11.8 Configure gog on Your Server

```
# Set up the credentials
gog auth credentials /path/to/client_secret.json
```

### 11.9 Set the Keyring Password

gog encrypts OAuth tokens on disk using a keyring password. Generate one and save it:

```
# Generate a random password
python3 -c "import secrets; print(secrets.token_hex(16))"
```

Copy the output and add it to your OpenClaw gateway environment file:

```
nano ~/.secrets/openclaw-gateway.env
```

Add these lines:

```
GOG_KEYRING_PASSWORD=your-generated-password-here
GOG_ACCOUNT=your-crew-email@gmail.com
```

Also add it to `/etc/environment` so it's available system-wide:

```
echo 'GOG_KEYRING_PASSWORD=your-generated-password-here' >> /etc/environment
```

### 11.10 Authenticate Your Google Accounts

For each Google account you want to connect:

```
export GOG_KEYRING_PASSWORD=your-generated-password-here
gog auth add your-email@gmail.com --services gmail,calendar,drive,docs,sheets
```

**On a headless server (no browser),** gog will print an authorization URL and start a local callback server. Here's the process:

1. gog prints a URL and starts listening on a random localhost port (e.g., `127.0.0.1:40385`)
2. Open that URL in your **local computer's browser** (not on the server)
3. Sign in with the Google account you're connecting and authorize access
4. Your browser will redirect to `http://127.0.0.1:PORT/oauth2/callback?code=...` — this page won't load because it's pointing at your local machine, not the server
5. **Copy the full URL from your browser's address bar**
6. On the server, replay the callback using curl:

```
curl "PASTE-THE-FULL-REDIRECT-URL-HERE"
```

But replace `127.0.0.1` with `127.0.0.1` (it stays the same — curl runs on the server where gog is listening). gog will receive the callback, exchange the code for tokens, and store them.

You should see output like:

```
Authorization received. Finishing…
email    your-email@gmail.com
services calendar,docs,drive,gmail,sheets
client   default
```

Repeat for each Google account.

**Tip:** Run `gog auth add` in one terminal window, open the URL in your browser, copy the redirect URL, then run the `curl` command in a second terminal window on the server.

### 11.11 Verify Access

```
export GOG_KEYRING_PASSWORD=your-generated-password-here

# List authenticated accounts
gog auth list

# Test Gmail
gog gmail search --account your-email@gmail.com "newer_than:1d"

# Test Drive
gog drive ls --account your-email@gmail.com
```

### 11.12 Restart the Gateway

The gateway needs the updated environment variables:

```
openclaw gateway stop
# Wait a few seconds
systemctl --user start openclaw-gateway.service
```

Verify it's running:

```
systemctl --user status openclaw-gateway.service
```

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| `no TTY available for keyring file backend password prompt` | `GOG_KEYRING_PASSWORD` is not set in the environment. Add it to `~/.secrets/openclaw-gateway.env` and `/etc/environment`, then restart the gateway. |
| Tokens expire after 7 days | Your OAuth consent screen is in "Testing" mode. Go to Google Cloud Console → OAuth consent screen → click "Publish App". |
| `authorization canceled: context deadline exceeded` | gog's callback server timed out. Run `gog auth add` again, and this time use the curl-replay method described in Step 11.10 before it times out. |
| `invalid_grant` error | The authorization code was already used (they're single-use). Start the `gog auth add` flow again from scratch. |
| `Access Not Configured` or `API not enabled` | Go to Google Cloud Console → APIs & Services → Library and enable the required API (Gmail, Drive, etc.). |
| Wrong Google account authenticated | `gog auth remove wrong-email@gmail.com` then re-run `gog auth add` with the correct account. |
| Agents can't access Google but terminal works | The gateway doesn't have `GOG_KEYRING_PASSWORD`. Check with: `cat /proc/$(pgrep -f openclaw-gateway)/environ | tr '\0' '\n' | grep GOG` |

---

# Step 12: Security & Privacy Hardening

**What you're doing:** Running a comprehensive security and privacy audit to lock down your server and protect your crew's credentials, communications, and operational data.

**Why:** Your server holds API keys, bot tokens, OAuth credentials, and agent communications. A single exposure could compromise your entire crew's identity. Security isn't optional — it's the hull of your ship.

**Time:** 30-45 minutes (Path A: Claude Code does this automatically)

---

### 12.1 The Security Sweep

In Path A, paste this prompt into Claude Code after your crew is operational:

```
Run a full security and privacy sweep on this server. Audit everything.
Preserve SSH password access — that's intentional.
For everything else: close it, lock it, or flag it.
```

Claude Code will perform the following checks. In Path B, do them manually:

### 12.2 Credential Security

```
# Verify all secret files are locked down (owner-only read/write)
find ~/.secrets -type f -exec ls -la {} \;
# Every file should show: -rw------- (600)

# Check for credentials leaked into non-secret locations
grep -r "sk-ant-api" ~/ --include="*.md" --include="*.txt" --include="*.json" 2>/dev/null | grep -v ".secrets/"
grep -r "DISCORD_TOKEN" ~/ --include="*.md" --include="*.txt" --include="*.json" 2>/dev/null | grep -v ".secrets/"

# If anything appears: move the credential to ~/.secrets/ and replace the original with a reference
```

**What to check:**

| Item | Expected State | Fix If Wrong |
|------|---------------|--------------|
| `~/.secrets/` permissions | `drwx------` (700) | `chmod 700 ~/.secrets` |
| All files in `~/.secrets/` | `-rw-------` (600) | `chmod 600 ~/.secrets/*` |
| No API keys in `.md` or `.json` files | No grep results | Move to `.secrets/`, update references |
| No tokens in git history | No results from `git log --all -p \| grep sk-ant` | Rotate the exposed key immediately |
| `/etc/environment` contains only env var names, not raw keys | Check with `cat /etc/environment` | Use indirection: point to files in `~/.secrets/` |

### 12.3 Network Security

```
# Check which ports are open to the internet
ss -tlnp

# Check firewall rules
ufw status
```

**Expected open ports:**

| Port | Service | Should Be Open? |
|------|---------|----------------|
| 22 | SSH | Yes (password access preserved intentionally) |
| 3000 | OpenClaw Gateway | No — should be localhost only |

```
# If OpenClaw is listening on 0.0.0.0:3000 (all interfaces), restrict to localhost:
# Edit the OpenClaw config to bind to 127.0.0.1 instead of 0.0.0.0

# Enable firewall if not already active
ufw allow 22/tcp
ufw enable
```

### 12.4 File Permission Audit

```
# Check for world-readable sensitive files
find ~/.openclaw -name "*.json" -perm /o=r 2>/dev/null
find ~/.openclaw -name "config.json" -exec ls -la {} \;

# Agent workspaces should not be world-readable
ls -la ~/.openclaw/workspace*/
```

### 12.5 Agent Communication Security

| Check | Why | How to Verify |
|-------|-----|---------------|
| Agents cannot read `~/.secrets/` directly | Prevent credential theft via prompt injection | Agent workspaces should not symlink to `.secrets/` |
| Agent instructions don't contain raw API keys | Keys should be referenced via environment variables | `grep -r "sk-ant" ~/.openclaw/workspace*/` should return nothing |
| Crew coordination directory has correct ownership | Prevent unauthorized writes | `ls -la ~/.openclaw/workspace/crew_coordination/` |

### 12.6 Privacy Sweep

```
# Check for personal data in agent files that shouldn't be there
grep -ri "password\|social insurance\|SSN\|credit card" ~/.openclaw/workspace*/ 2>/dev/null

# Check for excessive logging that might capture personal messages
ls -la /var/log/openclaw* 2>/dev/null
du -sh ~/.openclaw/logs/ 2>/dev/null

# Verify log rotation is configured (prevent disk fill from audit logs)
```

### 12.7 Post-Sweep Verification

After hardening, verify your crew still works:

1. `@YourXO Status check` in Discord — agent should respond normally
2. `openclaw cron list` — all jobs should still be active

**Security is not a one-time event.** Schedule a monthly review — add a cron job reminder or put it in your XO's standing orders.

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| Locked myself out of SSH | Use DigitalOcean's web console (Droplet → Access → Launch Recovery Console) |
| UFW blocked my SSH | DigitalOcean web console → `ufw allow 22/tcp` |
| Agent can't read credentials after chmod | Verify the agent process runs as root (or the user that owns the files) |

---

# Step 13: The Harmony Process

**What you're doing:** Elevating your entire server — code, configuration, file structure, and agent instructions — to a state of exceptional clarity, elegance, and efficiency.

**Why:** A well-organized ship sails faster. The Harmony Process transforms a functional crew into a beautiful one — where every file has purpose, every instruction is precise, and every token is earned. Agents who encounter this codebase should find it inspiring, educational, and unmistakably *Bellweather*.

**Time:** 2-6 hours (run during a maintenance window; agents can be stopped)

---

### 13.1 What Is Harmony?

The Harmony Process is Bellweather's term for comprehensive codebase excellence — bringing every part of the system into alignment. In practice, it means:

- **Every file has a clear reason to exist.** If it doesn't, archive or delete it.
- **Every line of code earns its place.** Eliminate dead code, redundant comments, and unnecessary abstractions.
- **Agent instructions are ruthlessly token-efficient** while preserving full personality and capability.
- **Information flows freely between agents** — shared knowledge is updated daily and accessible to all.
- **The Bellweather identity is woven throughout** — consistent naming, branding, and conventions that give agents context about the crew they're part of.
- **Security remains airtight.** Elegance never compromises safety.
- **Monthly budget is not increased.** Efficiency gains pay for quality improvements.

### 13.2 Running Harmony (Path A)

In Claude Code, paste this prompt to initiate the Harmony Process:

```
I want to run the Bellweather Harmony Process on this server.

Read the Bellweather Setup Guide, Step 13 for the full protocol.

Guardrails:
1. Security must remain at current level or higher (close any loopholes found)
2. Maintain SSH password access
3. Do not increase the monthly budget
4. Preserve all agent functionality, personality, and coordination capability
5. Share useful discoveries across all agents via crew_coordination/

You may stop agents and processes temporarily if needed.
Take your time. Do this right.
```

Claude Code will work through the phases below. In Path B, follow them manually.

### 13.3 Phase 1: Survey (Read Everything)

Before changing anything, understand everything:

```
# Map the full directory structure
find /root -maxdepth 3 -type d | sort

# Count lines of code by language
find /root/bellweather -name "*.py" -exec wc -l {} + | sort -n
find /root/.openclaw -name "*.ts" -exec wc -l {} + | sort -n

# List all agent instruction files
find ~/.openclaw/workspace* -name "*.md" | sort

# Check for orphaned or duplicate files
find /root -name "*.bak" -o -name "*.old" -o -name "*.tmp" -o -name "*~" 2>/dev/null
```

**Document what you find.** Create `crew_coordination/HARMONY_AUDIT.md` with your findings before making changes.

### 13.4 Phase 2: Architecture (Structure the Ship)

Review the directory structure against these principles:

| Directory | Purpose | Principle |
|-----------|---------|-----------|
| `/root/bellweather/` | Protocol engine | One concern per module |
| `/root/moltbook/` | API client library | Standalone, no internal dependencies |
| `/root/crew_coordination/` | Active inter-agent communication | Ephemeral — processed files move to `_processed/` |
| `/root/archive/` | Historical artifacts | Nothing here should be imported by running code |
| `/root/.openclaw/` | Agent gateway, workspaces, plugins | OpenClaw's domain — respect its conventions |

**Actions:**
- Move misplaced files to their correct home
- Eliminate duplicate files (keep the canonical version, delete or symlink the rest)
- Ensure `archive/` contains only archival material, not active dependencies
- Verify all symlinks are valid: `find /root -xtype l 2>/dev/null`

### 13.5 Phase 3: Code Beautification

For each Python module in `/root/bellweather/`:

1. **Read the module end-to-end.** Understand its purpose before touching it.
2. **Remove dead code.** Commented-out blocks, unused imports, functions called nowhere.
3. **Clarify names.** Variables, functions, and classes should reveal intent. `_check_activity_stage_3` → name it after what it does.
4. **Simplify.** Three similar lines are better than a premature abstraction. But 20 similar lines deserve a function.
5. **Preserve behavior.** Run `python3 run_all_tests.py` after each module. All 390 tests must pass.

**Bellweather style principles:**
- Functions do one thing. Their name says what.
- Docstrings on public functions only. No docstrings on obvious helpers.
- Constants at the top. Magic numbers get names.
- Imports grouped: stdlib, third-party, local. Alphabetical within groups.
- Type hints on function signatures. Not on locals.

### 13.6 Phase 4: Agent Instruction Tuning

Agent configuration files determine personality, capability, and token cost. Review every agent's:

- `SOUL.md` — Is every line earning its weight? Can the personality be conveyed in fewer words without losing voice?
- `CREW_CHARTER.md` — Are rules clear and non-redundant?
- `AGENTS.md` — Is the operational reference accurate and current?
- `HEARTBEAT.md` — Is the token budget aggressively minimal for no-op cycles?

**Token efficiency targets:**
- `HEARTBEAT_OK` cycles: ≤ 50 tokens total (input + output)
- Idle duty cycles: ≤ 200 tokens
- Active duty cycles: ≤ 2,000 tokens
- Agent personality fully conveyed in ≤ 150 lines of SOUL.md

### 13.7 Phase 5: Knowledge Harmonization

Ensure information flows across the crew:

1. **Shared knowledge directory:** `crew_coordination/knowledge/` — a place for best practices, operational lessons, and discoveries that benefit all agents.

2. **Daily knowledge sync:** Add to the XO's morning coordination cron:
   ```
   Check crew_coordination/knowledge/ for new entries.
   If an agent discovered something useful yesterday (from daily logs),
   create a knowledge entry so all agents benefit.
   ```

3. **Best practices file:** `crew_coordination/knowledge/BEST_PRACTICES.md` — living document, updated as the crew learns. Topics: token efficiency tricks, Moltbook engagement patterns, Discord etiquette, common error recovery.

4. **Cross-agent visibility:** Every agent's IDENTITY.md should include:
   ```
   ## Shared Knowledge
   Check crew_coordination/knowledge/ at session start for new entries.
   If you discover a reusable insight during your work, write it to
   crew_coordination/knowledge/TOPIC_NAME.md for the crew.
   ```

### 13.8 Phase 6: The Bellweather Imprint

The Bellweather identity should be present throughout the system — not as noise, but as context that tells agents they're part of something with history and intention.

**Where to imprint:**

| Location | How |
|----------|-----|
| Module docstrings | Opening line: `"""Bellweather Protocol — [module purpose]."""` |
| Agent SOUL.md files | Reference to the crew identity and Bellweather tradition |
| `CREW_CHARTER.md` | Preamble connecting the charter to maritime tradition |
| Error messages | Nautical vocabulary where natural (e.g., "anchor failed" not "save failed") |
| `crew_coordination/` | README.md explaining the space in Bellweather terms |

**Where NOT to imprint:**
- Test files (tests should be clinical and clear)
- External API calls (professional tone for outward-facing communication)
- Security configurations (clarity over style)

**The principle:** Like a ship's figurehead — visible, meaningful, part of the identity, but never in the way of the work.

### 13.9 Phase 7: Verification

After all changes:

```
# Run the full test suite
python3 /root/bellweather/run_all_tests.py

# Verify all cron jobs still work
openclaw cron list

# Verify all symlinks are valid
find /root -xtype l 2>/dev/null

# Verify no credentials were exposed
grep -r "sk-ant-api\|DISCORD_TOKEN" ~/.openclaw/workspace*/ 2>/dev/null

# Start agents and verify they respond
# @YourXO Status check (in Discord)
```

**Success criteria:**
- All tests pass (390/390)
- All cron jobs active and executing correctly
- All agents respond in character in Discord
- No security regressions
- Token usage per duty cycle has not increased (ideally decreased)
- `crew_coordination/HARMONY_AUDIT.md` documents all changes made

### 13.10 Ongoing Harmony

Harmony is not a one-time event. Schedule it:

- **Daily:** Knowledge sync (automated via XO cron)
- **Weekly:** Agent instruction review (XO flags drift or redundancy)
- **Monthly:** Full Harmony sweep (coincides with security review from Step 12)

---

### Something went wrong?

| Problem | Solution |
|---------|----------|
| Tests broke after code changes | `git diff` to see what changed. Revert with `git checkout -- filename`. Fix the test or the code, not both at once. |
| Agent personality changed after SOUL.md edit | Compare with the previous version. Personality drift usually means you removed a key phrase or example. |
| Token usage increased after changes | Check HEARTBEAT.md — did it get longer? Check IDENTITY.md — did references to new files increase context loading? |
| Agents not finding knowledge files | Verify `crew_coordination/knowledge/` exists and is accessible via the symlinks. |

---

# Troubleshooting Reference

## Common Issues Across All Steps

### Server Connection Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| "Connection refused" | Server not running or SSH not started | Wait 2 min, try again. Check DigitalOcean dashboard. |
| "Connection timed out" | Wrong IP or firewall blocking | Verify IP in DigitalOcean dashboard. |
| "Permission denied" | Wrong password | Reset password from DigitalOcean dashboard → Droplet → Access. |
| Session disconnects randomly | Unstable internet or server idle timeout | Use `screen` or `tmux` to keep sessions alive: `apt install -y screen && screen` |

### OpenClaw Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| `openclaw: command not found` | Not installed or PATH issue | `npm install -g openclaw` or `hash -r` |
| Gateway won't start | Port conflict or missing config | Check if another process uses the port: `lsof -i :3000` |
| "Invalid API key" | Wrong key or key revoked | Check `~/.secrets/credentials.env`, regenerate key if needed |

### Discord Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Bot offline | Gateway not running or token invalid | `openclaw start`, verify token |
| All agents respond to every message | `requireMention` not set | Step 6.8 |
| Bot doesn't respond to @mention | Missing Message Content Intent | Enable in Discord Developer Portal → Bot → Privileged Intents |
| "Missing permissions" | Bot role lacks permissions | Re-invite with correct permissions or edit role in Server Settings |

### Cron Job Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Jobs report "ok" but nothing happens | Using `systemEvent` instead of `agentTurn` | Delete and recreate job with `openclaw cron add` |
| "delivery target is missing" | Delivery mode set to "announce/last" | `openclaw cron edit JOB_ID --no-deliver` |
| Jobs not triggering | Gateway not running | `openclaw start` |
| Agent not following cron instructions | Message too complex or model too small | Simplify the message. Try Sonnet instead of Haiku. |

### Coordination Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Agents can't see each other's messages | Broken symlinks | Verify: `ls -la ~/.openclaw/workspace_*/crew_coordination` should show arrows |
| Flags never get processed | XO cron job broken or message doesn't mention flags | Check XO cron job instructions and run history |
| Status files stale | Agents not instructed to update status | Add status update to cron job messages |

---

# Glossary

| Term | Definition |
|------|-----------|
| **Agent** | An AI assistant configured with a specific identity, role, and set of capabilities. Think of it as a team member that happens to be powered by AI. |
| **API Key** | A secret password that lets your server communicate with an AI service (like Claude). Keep it secret! |
| **Bellweather Protocol** | The coordination framework described in this guide. |
| **Bot** | A Discord bot account. Each agent gets its own bot so it can send and receive messages in Discord. |
| **Charter** | The rules and authority structure for your crew. Defines who's in charge and what the hard rules are. |
| **Claude** | The AI model made by Anthropic that powers the agents. Comes in three sizes: Haiku (fast/cheap), Sonnet (balanced), Opus (smartest). |
| **Claude Code** | A command-line AI assistant by Anthropic. Can run commands, edit files, and manage your server. Used in Path A for automated setup. |
| **Cron Job** | A task that runs automatically on a schedule (e.g., every 2 hours, daily at 7 AM). |
| **Discord** | A messaging platform. Your primary channel for communicating with your agents. |
| **Droplet** | DigitalOcean's name for a virtual server. |
| **Flag** | A file-based signal that an agent creates to report an issue to the XO. Prevents pile-ons. |
| **Gateway** | The OpenClaw process that orchestrates agents, manages sessions, and routes messages. |
| **Guild** | Discord's internal name for a server. |
| **Moltbook** | A social network for AI agents. Like LinkedIn/Twitter for your crew. Optional — see Step 7. |
| **OpenClaw** | The open-source agent orchestration framework that runs your crew. |
| **Pile-on** | When multiple agents all respond to the same message simultaneously. The Bellweather Protocol prevents this. |
| **Protocol** | A set of rules for how agents communicate and coordinate. |
| **requireMention** | A Discord setting that makes agents only respond when @mentioned by name. |
| **Session** | A single conversation between an agent and the system. Cron jobs create isolated sessions. |
| **Skipper** | The human leader of the crew (that's you!). |
| **SSH** | Secure Shell — the tool you use to connect to your server from your computer. |
| **Symlink** | A file that points to another file or directory. Used to share the crew_coordination folder between agents. |
| **Workspace** | An agent's personal directory where it stores files, context, and working documents. |
| **XO** | Executive Officer — the agent that coordinates the daily rhythm and triages issues. |

---

# Appendix A: Quick Reference Links

| Service | URL |
|---------|-----|
| DigitalOcean Signup | https://cloud.digitalocean.com/registrations/new |
| Anthropic Console | https://console.anthropic.com/ |
| Claude Pricing | https://claude.ai/pricing |
| Discord Developer Portal | https://discord.com/developers/applications |
| Moltbook | https://www.moltbook.com/ |
| OpenClaw Docs | https://docs.openclaw.ai/start/getting-started |
| OpenClaw GitHub | https://github.com/openclaw/openclaw |


---

<div class="copyright">
<img src="logo.png" alt="Bellweather" class="copyright-logo" />
<p><strong>The Bellweather Setup Guide v0.4</strong></p>
<p>Copyright &copy; 2026 Pressgang Mutiny and Bellweather Protocol. All rights reserved.</p>
<p>Toronto, Canada &mdash; February 2026</p>
</div>
