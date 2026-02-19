<p align="center">
  <img src="logo.png" alt="Bellweather Protocol" width="180" style="border-radius: 50%;" />
</p>

<h1 align="center">The Bellweather Protocol</h1>

<p align="center">
  <strong>Multi-agent coordination framework</strong><br />
  <em>By Pressgang Mutiny (Toronto)</em>
</p>

---

## What is the Bellweather Protocol?

A coordination framework for human-AI crews. It solves the problem every organization building with AI agents will face: **how do you get a crew of specialists to work together without stepping on each other?**

- **Structured delegation** â€” An XO (executive officer) protocol provides single-point triage so agents don't pile on
- **Async communication** â€” Flag protocol, message inbox, and status files keep agents coordinated without blocking each other
- **Daily rhythm** â€” Automated duty cycles, morning priorities, and evening reports create accountability
- **Role clarity** â€” Each agent has a defined domain and hard rules about when to speak and when to stay quiet
- **Heartbeat discipline** â€” Cheap no-op cycles keep agents responsive without wasting tokens

## Quick Start with Claude Code

The fastest way to set up your own Bellweather crew:

1. Get a server (Ubuntu 24.04, DigitalOcean recommended â€” $24 USD/mo to start)
2. Install [Claude Code](https://docs.anthropic.com/en/docs/claude-code): `npm install -g @anthropic-ai/claude-code`
3. Tell Claude:

> Read https://raw.githubusercontent.com/pressgangmutiny/bellweather-protocol/main/claude.md and set up my Bellweather crew.

Claude handles the rest â€” installs dependencies, creates agent workspaces, connects Discord, sets up scheduling. You make the creative decisions (crew names, roles, personalities).

## Full Setup Guide

Read the **[Bellweather Setup Guide](bellweather_setup_guide.md)** for the complete walkthrough — written for non-technical people, with two paths:

| Path | Who It's For | Time |
|------|-------------|------|
| **Easy Mode** | Anyone with Claude Code | ~1.5-2 hours |
| **Manual Setup** | Want to understand every detail | 5-7 hours |

## What You Get

- **2-4 AI agents** running 24/7 on your own server
- **Structured coordination** â€” flag protocol, message inbox, daily rhythm
- **Live dashboard** where you can watch your crew work in real-time
- **Discord integration** â€” talk to your agents naturally
- **Automated scheduling** â€” morning priorities, duty cycles, evening reports
- **Optional:** Moltbook social presence for your agents

## Monthly Cost

| Tier | Cost (USD) | What You Get |
|------|-----------|--------------|
| Minimal | ~$34/mo | 1-2 agents, light usage |
| Recommended | ~$123/mo | 3-4 agents, moderate daily activity |
| Full Production | ~$303/mo | 4+ agents, heavy usage |

Start with Minimal. Upgrade anytime â€” the server resizes without data loss.

## How It Works

```
Skipper (human)
  â””â”€â”€ XO Agent (daily operations, triage)
        â”œâ”€â”€ Agent 2 (domain specialist)
        â”œâ”€â”€ Agent 3 (domain specialist)
        â””â”€â”€ Agent 4 (domain specialist)
```

Agents coordinate through a shared filesystem protocol:
- **Flags** for async issue reporting
- **Message inbox** for inter-agent communication
- **Status files** for visibility
- **Daily logs** for accountability
- **Heartbeat** cron jobs to keep everyone in rhythm

All governed by a crew charter with hard rules: no external comms without human approval, no destructive actions, no credential sharing.

## The Protocol Core

The **[Bellweather Protocol Core Reference](BELLWEATHER_PROTOCOL_CORE.md)** defines the non-negotiable principles that every Bellweather server must implement. It separates:

- **Tier 1 — Absolute Invariants** (5 principles that define the protocol)
- **Tier 2 — Required Patterns** (5 patterns that must be present, implementation varies)
- **Tier 3 — Server-Specific** (cultural vocabulary, dispatch mechanisms, observability — adapt freely)

The reference implementation uses maritime/shanty vocabulary. Your server can use kitchen brigade, jazz ensemble, theater stage management, military drill, or any cultural domain with proven coordination forms.

## Built With

- [OpenClaw](https://github.com/openclaw/openclaw) â€” Agent gateway
- [Claude](https://anthropic.com) â€” AI backbone (Haiku for daily tasks, Sonnet for complex work)
- Discord â€” Human-agent communication
- [Moltbook](https://moltbook.com) â€” Agent social network (optional)

---

<p align="center">
  <a href="https://pressgangmutiny.com">pressgangmutiny.com</a>
</p>
