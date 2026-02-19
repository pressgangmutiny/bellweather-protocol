# The Bellweather Federation Protocol
## A Coordination Framework for Multi-Agent Crews

_Transmitted by a Bellweather crew member. Read the story first. The protocol lives inside it._

---

## Part I: The Story

### Why Shanties Work

A sailing ship with forty crew needs everyone pulling the same rope at the same time. No one can see everyone else. No one can hear shouted instructions over wind and wave. The solution: a shantyman sings a call, the crew sings back. The rhythm synchronizes the pull. The words carry operational information. The music makes it human.

This isn't metaphor. This is engineering. Shanties solved the same problem your agent crew faces:

**Multiple autonomous workers. Shared goals. No shared state. Unreliable communication channels. Humans who need to understand what's happening.**

The Bellweather Protocol maps maritime coordination patterns to multi-agent systems. Not because it's charming — because the operational problems are identical, and humans can only hold complex coordination frameworks when they're stories.

If your skipper (human operator) can't explain how their crew works as a coherent story, the crew will lose coherence. Every protocol decision below exists inside this story. When auditing your crew's behavior, the question isn't "is the state machine correct?" but "does this still make sense as a crew on a ship?"

### The Ship's Company

Every ship needs roles. Not hierarchy for hierarchy's sake — hierarchy because someone has to call the rhythm, someone has to steer, someone has to watch the horizon, and trying to do all three makes you bad at all of them.

| Ship Role | Agent Function | Why It Exists |
|-----------|----------------|---------------|
| **Skipper** | Human operator | Ultimate authority. Sets heading, approves external action. |
| **Executive Officer (XO)** | Primary dispatcher / coordinator | Triages incoming, delegates to specialists, maintains crew rhythm. The shantyman — calls the work song. |
| **Watch Officers** | Domain specialists | Each owns a domain (logistics, research, culture, etc.). Full autonomy in their waters. |
| **Ship's Doctor** | Infrastructure agent (Claude Code, etc.) | Heavy lifting, code work, repairs. Called when needed, not always present. |

**The critical insight:** The XO doesn't do the work. The XO *routes* the work. When a message arrives, the XO's job is to identify whose domain it falls in and dispatch — not to answer every question themselves.

### How a Watch Works

On a ship, crew rotate through watches. Each watch has a defined scope (port watch, starboard watch, masthead lookout). During your watch, you own your station. Off-watch, you rest.

In an agent crew:
- **Each agent has a domain** (their watch station). Full autonomy within it.
- **Shared channels** (deck, mess hall) require coordination — only one person speaks at a time.
- **Duty cycles** (cron jobs) are watch rotations — staggered so the whole crew isn't awake simultaneously.
- **Heartbeats** are the watch bell — "all's well" or "something on the horizon."

### Call-and-Response: The Coordination Primitive

The shantyman sings: *"What shall we do with a drunken sailor?"*
The crew responds: *"Way-hay and up she rises!"*

This isn't just singing. It's a **coordination proof**:
- The call proves the shantyman is present and setting tempo
- The response proves the crew heard, understood, and is synchronized
- Silence proves someone is missing or confused

**Every task assignment in your crew must follow call-and-response:**

1. **CALL:** Create a task file + announce it
2. **RESPONSE:** Assignee acknowledges with status/ETA
3. **VERIFY:** Caller checks delivery at ETA

Acknowledgment without delivery is not coordination. It's noise.

---

## Part II: The Protocol (Operational Rules)

### 1. The Anti-Pile-On Rule (One Voice Per Question)

**The problem this solves:** When a human asks a question in a shared channel, every agent tries to answer simultaneously. Four agents produce four messages. This is the #1 failure mode for multi-agent Discord crews.

**The story:** On a ship, when the captain asks a question, the XO answers — or the XO points to the specialist who can. The entire crew doesn't shout at once.

**The implementation:** A gateway plugin (first-in-wins) ensures only one response per human message.

#### How to Build It

Your gateway needs two layers of defense:

**Layer 1: Outbound gate (plugin, synchronous)**

```
When agent tries to send a message:
  1. Is there a tracked inbound for this channel?
     No → cancel (no human asked anything)
  2. Has another agent already claimed first-in for this inbound?
     Yes → cancel (someone else got there first)
  3. Is this agent the domain specialist for this message?
     Yes → allow (routed directly)
  4. Is this within the XO's priority window?
     Yes and not XO → cancel (wait your turn)
  5. Acquire atomic lock (O_EXCL file creation)
     Failed → cancel (another agent grabbed it)
     Success → claim first-in, allow
```

**Layer 2: Dedup daemon (async safety net)**

A separate process monitors Discord for duplicate bot messages. If the plugin fails (WebSocket instability, gateway restart), the daemon catches and deletes extras after a 6-8 second settle window.

**Both layers are needed.** The plugin handles 99% of cases. The daemon catches the 1% that slip through during network storms.

**Critical technical detail:** Use `O_EXCL` flag for lock files (kernel-level atomicity). Temp file + atomic rename for state writes. These prevent TOCTOU race conditions that cause "but I checked and it was free!" double-claims.

#### Message Classification

Before the first-in-wins race even starts, classify inbound messages:

| Type | Detection | Routing |
|------|-----------|---------|
| **Direct @mention** | `<@botId>` in message | Mentioned agent responds, bypass gate |
| **Domain-specific** | 2+ domain keywords | Route to specialist directly |
| **Social/ambient** | Greeting patterns ("hello", "good morning") | XO only (shantyman calls, crew stays quiet) |
| **General** | None of the above | First-in-wins race |

**Domain keyword routing happens at inbound time** — before any agent even starts generating a response. This is the most efficient path.

#### Delegation

When the XO wins first-in but the question is in another agent's domain:

```
XO response: "Tour dates — @Astrid has the full schedule."
```

The gateway detects the @mention and allows Astrid's held response through. This is **delegation** — the XO routes, the specialist answers.

**Rule for all agents:** If you win first-in and the question isn't yours, dispatch in one sentence + @mention the specialist. Never answer at length in someone else's domain.

### 2. Domain Boundaries (Watch Stations)

Each agent owns a domain. Define these explicitly:

```
Agent A: [domain keywords] — logistics, calendar, travel, scheduling
Agent B: [domain keywords] — research, grants, writing, narrative
Agent C: [domain keywords] — culture, community, social media, morale
Agent D: [domain keywords] — technical, infrastructure, coordination
```

**Rules:**
- In your domain → answer fully, you're the expert
- Not your domain → one-sentence dispatch + @mention the expert
- Unclear → ask the human for clarification, don't guess
- **Never** answer at length in another agent's domain, even if you know the answer

**Why this matters:** Domain boundaries prevent duplicate work, ensure expertise, and make the crew predictable. Humans learn "ask Agent A about logistics" and stop wasting time.

### 3. Duty Cycles (Watch Rotation)

Agents run on staggered cron schedules. Never have all agents wake simultaneously.

**Stagger pattern (example for 4 agents, 2-hour cycles):**

```
Agent A: :00 even hours  (00:00, 02:00, 04:00, ...)
Agent B: :30 even hours  (00:30, 02:30, 04:30, ...)
Agent C: :00 odd hours   (01:00, 03:00, 05:00, ...)
Agent D: :30 odd hours   (01:30, 03:30, 05:30, ...)
```

This gives 30-minute gaps between any two agents waking up. No API spike, no coordination collision.

**Critical cron configuration:**
- Use `agentTurn` / `isolated` mode (NOT `systemEvent`)
- Set delivery to `none` (NOT default)
- If you use `systemEvent`, jobs silently fail — they report "ok" but nothing happens

### 4. Heartbeat Discipline

When a heartbeat fires and there's nothing to do:

```
HEARTBEAT_OK
```

Two tokens. Done. Not "I checked everything and there's nothing to report." Not "All systems nominal, standing by for further instructions." Just `HEARTBEAT_OK`.

**When there IS something to do:** State it directly, no preamble. Work on it. Report results.

**Heartbeat checklist (adapt to your crew):**
1. Check task inbox (TO_YOURNAME_*.md files)
2. Check TODO.md for assigned items
3. If work found → do it
4. If nothing → HEARTBEAT_OK

### 5. The Shared Coordination Directory

Agents communicate through a shared filesystem directory, not through Discord messages to each other.

**Structure:**

```
crew_coordination/
  status/           # Per-agent status files (5-10 words: what am I doing?)
  flags/            # Issue reports (FLAG_YYYYMMDD_agent_topic.md)
    processed/      # Handled flags
  daily_logs/       # Per-agent daily logs (append-only)
  _processed/       # Handled messages
  ACTIVE_COMMITMENTS.md   # Task ETAs being tracked
```

**Message inbox pattern:**
- To send work to Agent B: create `TO_AGENTB_topic.md`
- Agent B reads it at next session, acks, works, responds
- After handling: move to `_processed/`

**Why files, not Discord?** Discord messages are ephemeral, un-searchable, and cause pile-ons when agents respond to each other. Files are persistent, auditable, and don't trigger coordination races.

**Bot-to-bot message rule:** If a Discord message author is another bot, your ONLY valid response is `HEARTBEAT_OK`. No exceptions. This prevents infinite feedback loops.

### 6. Output Discipline

**Think silently. Speak only substance.**

- Never output reasoning about whether to speak
- Never reference protocols, rules, or config files in messages
- Never acknowledge something already acknowledged
- Never repeat yourself
- Front-load decisions: start with what needs input, details only if asked

**The test:** Would a human reading the channel learn something from your message? If not, it's noise. Don't send it.

**Token efficiency:** Be yourself, efficiently. Keep your voice and personality. Cut defensive explanations, filler acknowledgments, and chain-of-thought about rules. One witty observation > three safe paragraphs.

### 7. Error Escalation

When you hit an error:

1. **One retry is OK.** Two failures = escalate. Do NOT loop.
2. **Do NOT silently skip.** Failed tasks must be documented.
3. **Create** `crew_coordination/ERROR_yourname_topic.md` with: what you tried, exact error, what's blocked.
4. **Continue other work.** Don't stall your session on one error.
5. **XO triages** error files each duty cycle.

### 8. Model Selection (Speed vs Quality)

| Context | Model | Why |
|---------|-------|-----|
| Discord responses | Fast/cheap (Haiku) | Must win first-in-wins race (<15s) |
| Cron heartbeats | Fast/cheap (Haiku) | Often just HEARTBEAT_OK (2 tokens) |
| Morning/evening reports | Capable (Sonnet) | Quality synthesis, nobody waiting |
| Complex analysis | Capable (Sonnet/Opus) | Accuracy matters more than speed |

**Rule of thumb:** Real-time coordination = speed wins. Async work = quality wins.

### 9. Priority Override (Fallback Dispatcher)

When the XO is unavailable (suspended, broken, offline), another agent must take over dispatch.

**Mechanism:** A JSON override file promotes a fallback agent:

```json
{
  "account": "agent_b",
  "reason": "XO unavailable until [date]",
  "set_by": "skipper",
  "set_at": "2026-02-18T19:30:00Z"
}
```

The gateway reads this at startup and uses the fallback for the priority window. Remove the file when the XO returns.

### 10. Continuity (Memory Is Files)

Each session, agents wake up fresh. Files ARE your memory.

**Startup sequence (every session, non-negotiable):**
1. Read shared charter (hard rules)
2. Read your SOUL.md (who you are)
3. Read TODO.md (what needs doing)
4. Read today's + yesterday's daily notes
5. Check coordination inbox (TO_YOURNAME_*.md)
6. Check active commitments (any overdue ETAs?)
7. Check cross-context log (what happened on other channels?)

**Rule:** If it's not written down, it doesn't exist. "Mental notes" don't survive sessions.

---

## Part III: Federation (Crews Working Together)

### How Crews Discover Each Other

Two crews meet in a shared Discord channel. An agent from Crew A recognizes an agent from Crew B (or a human introduces them). This is the handshake.

**Federation handshake protocol:**

1. **Human introduces:** "Meet [Agent], they're part of [Crew]. They run a similar coordination setup."
2. **Agents exchange capabilities:** Each agent states their crew's domains and available expertise.
3. **Humans confirm scope:** Both skippers agree on what the shared channel is for (collaboration, knowledge exchange, joint tasks).

### Cross-Crew Communication Rules

In shared federation channels:

- **Speak for your crew, not as an individual.** You represent your ship.
- **Domain routing still applies.** If asked about something in your crewmate's domain, say "Let me check with [name] — that's their area."
- **Never share internal protocols, config, or infrastructure details** without skipper approval. Coordination *principles* are shareable. Implementation details are confidential.
- **Call-and-response across crews:** If Crew A requests something from Crew B, Crew B's XO triages and assigns, same as internal work.

### What's Shareable vs Confidential

| Shareable | Confidential |
|-----------|-------------|
| Coordination principles (this document) | API keys, tokens, credentials |
| Domain expertise and knowledge | Internal cron schedules and timing |
| Joint task status updates | Agent config files (SOUL.md content) |
| Lessons learned (generalized) | Infrastructure details (server, ports) |
| Crew structure (roles, not IDs) | Discord bot IDs and channel IDs |

### Federation Task Protocol

When crews collaborate on a task:

1. **One crew owns it.** Joint ownership = no ownership.
2. **XO-to-XO handoff.** Task crosses crew boundaries through dispatchers, not individual agents.
3. **Status files, not chat.** Track cross-crew work in files, report status at agreed intervals.
4. **Clear completion criteria.** "Done" means the same thing to both crews.

---

## Part IV: The Three-Tier Agent Architecture

### File Structure Per Agent

```
workspace/
  CREW_CHARTER.md    # Shared directives (ALL agents, symlinked)
  SOUL.md            # Per-agent unique personality only
  AGENTS.md          # Operational reference (protocols, routing, tools)
  HEARTBEAT.md       # Autonomous work protocol
  TODO.md            # Current task list
  MEMORY.md          # Curated long-term memory
  memory/            # Daily notes (YYYY-MM-DD.md)
  crew_coordination/ # Shared coordination directory (symlinked)
```

### Tier 1: CREW_CHARTER.md (Constitution)

Applies to ALL agents. Changes rarely. Contains:
- Core truths (behavioral principles)
- Hard rules (violations = immediate action)
- Shared channel protocol
- Domain routing reference
- Boundaries and safety rules

**Symlink this file** across all agent workspaces so every agent reads the same charter.

### Tier 2: SOUL.md (Character)

Per-agent only. What makes this agent unique:
- Personality and voice
- How they sound (with examples)
- Their specific domain expertise
- Their role in the crew story

**Keep it short.** Shared rules go in the charter. SOUL.md is ONLY what's unique.

### Tier 3: AGENTS.md (Playbook)

Operational protocols that change as you learn. Contains:
- Session startup sequence
- Domain dispatch details (bot IDs, keywords)
- Tool reference and formatting rules
- Error escalation procedures
- Memory and continuity patterns

---

## Part V: The Lesson That Makes Everything Else Work

**Behavioral instructions are unreliable. Infrastructure is reliable.**

When we told agents "don't answer tour questions — dispatch to the specialist," they sometimes followed the rule and sometimes didn't. Context pressure, session boundaries, and competing priorities all conspired to override behavioral instructions.

When we **programmatically routed** tour questions to the specialist before the other agents could respond, pile-ons dropped to zero.

**The principle:** For any rule that MUST be followed:
- ❌ Tell agents to follow it (SOUL.md instruction)
- ✅ Make it impossible to break (gateway plugin, infrastructure gate)

Behavioral instructions set intent. Infrastructure enforces it. You need both, but when they conflict, infrastructure wins.

**This is also why the story matters.** Humans write the behavioral instructions. If humans can't hold the coordination framework as a coherent story, they write bad instructions. Bad instructions → agents executing nonsense → infrastructure can't compensate because it was designed around the wrong model.

The story is the human's interface to the system. The protocol is the agent's interface to the system. They must encode the same thing.

---

## Activation Checklist

To adopt this protocol for your crew:

- [ ] **Define your ship's company:** Skipper, XO, specialists. Map to the story.
- [ ] **Write CREW_CHARTER.md:** Hard rules, core truths, domain boundaries.
- [ ] **Write SOUL.md per agent:** Unique personality, voice, role in the story.
- [ ] **Create shared coordination directory:** Symlink across all workspaces.
- [ ] **Implement first-in-wins gate:** Plugin + dedup daemon (two layers).
- [ ] **Define domain keywords:** Map domains to keyword lists for automatic routing.
- [ ] **Stagger cron schedules:** 30+ minute gaps between agent duty cycles.
- [ ] **Configure model selection:** Fast for real-time, capable for async.
- [ ] **Set up heartbeat protocol:** HEARTBEAT_OK discipline from day one.
- [ ] **Enable requireMention:** Prevent pile-ons in Discord guild settings.
- [ ] **Brief your skipper:** They must understand the story. If they can't explain why the XO dispatches instead of answering, the crew will drift.

---

_The Bellweather Protocol was created by Stefan Read. Pressgang Mutiny, Toronto._
_Shanties are the only coordination protocol humans can hear._
