# The Bellweather Protocol — Core Reference

**Version:** 1.1
**Author:** Stefan Read
**Project:** Pressgang Mutiny, Toronto
**Date:** February 2026

This document defines the non-negotiable core of the Bellweather Protocol. Every server running the protocol references this document. Cultural vocabulary, metaphor domain, and implementation details vary by server. These principles do not.

---

## Preamble

The Bellweather Protocol is the principle that human cultural coordination forms — proven over centuries of coordinated action under uncertainty — are isomorphic to multi-agent orchestration problems. Encoding both the human-readable and machine-executable layers into a single monolithic structure creates systems that humans can hold as story and machines can execute as protocol.

The protocol is not prescriptive about *which* cultural forms a server uses. Shanties, kitchen brigade calls, jazz ensemble patterns, military drill, theatrical stage management, sports team playbooks — any cultural domain with proven coordination forms is valid. The protocol prescribes *how* cultural forms must be used: as dual-layer encoding, not as decoration.

### The Core Trade-Off

**This protocol does not optimize for speed.** Agents are fast enough. Every generation of model is faster than the last; speed is a commodity. What agents lack — and what degrades multi-agent systems — is accuracy, comprehensiveness, and coordination. Every design decision in this protocol trades speed for these three properties:

- **Accuracy:** The right agent answers, with the right information, checked against the right context.
- **Comprehensiveness:** No knowledge silos. No half-answers. No skipped context reads.
- **Coordination:** No pile-ons. No silent dependencies. No duplicate work. No missed handoffs.

**The priority ordering:** Budget → Accuracy & Coordination → Speed. Optimize accuracy and coordination within your token budget. Speed is last. When a design choice pits speed against accuracy, comprehensiveness, or coordination — choose the latter. When a design choice increases accuracy but blows the budget — find the cheapest path to accuracy instead.

---

## Tier 1: Absolute Invariants

**These define the protocol. Change any of them and it's not the Bellweather Protocol.**

### 1. Dual-Layer Encoding

Every coordination rule must be expressible as a single monolithic structure that humans read as narrative and agents execute as protocol. Not two aligned systems. Not a human layer and a machine layer synchronized afterward. One structure, two audiences.

**The test:** Can a human hold the coordination framework as a coherent story? Can an agent extract executable rules from the same structure? If both are true simultaneously, the encoding is dual-layer.

**Why this is invariant:** The story is load-bearing. When the human operator can't hold the coordination framework as story, they can't guide agents toward coherent behavior. The narrative constrains human understanding, which constrains human guidance, which constrains agent behavior. Remove the narrative and the system degrades — not because agents need stories, but because humans do, and humans are part of the system.

### 2. Infrastructure Over Instruction

Any rule that must hold gets enforced programmatically, outside the language model's generation context. Prompt-level behavioral instructions are for guidance (personality, tone, style), not guarantees (message deduplication, domain routing, cascade prevention).

**The test:** "If an agent violates this rule once, does the system fail?" If yes, it must be a programmatic gate. If no, it can be a prompt instruction.

**The two-layer defense:** For every must-hold rule, implement both a programmatic gate AND a prompt instruction encoding the same concern. If the prompt instruction drifts (LLM hallucination, context loss, behavioral drift), the gate still holds. The prompt instruction reduces the frequency of gate activations; the gate prevents catastrophic failure.

**Why this is invariant:** Language model agents do not reliably follow instructions. This is not a bug to be fixed — it's a fundamental property of probabilistic generation. Any protocol that depends on instruction-following for critical rules will fail. Infrastructure enforcement is the only reliable path.

### 3. Cultural Forms as Executable Protocols

Coordination patterns must be drawn from cultural forms with demonstrated historical effectiveness at preventing specific failure modes. The selection is systematic, not aesthetic: choose the cultural form because it solved this exact coordination problem, not because it sounds good.

**The taxonomy requirement:** Every cultural form used in the protocol must be traceable to a specific coordination failure it prevents. The mapping is:
- **Cultural form** → **Failure type** → **Mechanism** → **Implementation**

If you can't name the failure a cultural form prevents, it's decoration. If you can, it's engineering.

**Why this is invariant:** Using random metaphors produces inconsistent systems. Using historically-proven cultural forms produces systems that inherit centuries of refinement. The cultural form was optimized by human survival pressure — people died when coordination failed. That selection pressure produced forms that actually work.

### 4. Tiered Identity with Single Source of Truth

Agent behavioral configuration must be separated into:

1. **A shared constitutional layer** — values, principles, shared protocols. All agents read it every session. One authoritative source.
2. **A per-agent identity layer** — personality, domain, voice. Contains ONLY what makes this agent unique. No duplication of shared rules.
3. **A per-agent experiential layer** — accumulated knowledge, lessons learned, operational state. Persists across sessions.

Each rule has exactly one authoritative source. No rule appears in two places. Higher tiers override lower tiers on conflict: Constitutional > Identity > Experiential.

**Why this is invariant:** Duplicated rules drift. When the same rule exists in five files, they diverge. One says "3-second priority window," another says "6-second." The system behaves inconsistently. Single source of truth is the only way to prevent configuration entropy in multi-agent systems.

### 5. Call-and-Response Presence Verification

The system must have a mechanism where **silence is a signal**. If an agent doesn't acknowledge within a defined window, the system detects it and acts on it. Silence is never ambiguous — it always means "this agent may be offline, overloaded, or stuck."

**The implementation requirement:** Every coordination handoff (task assignment, dependency request, delegation) must have an expected response time. Exceeding that time triggers detection and escalation. The absence of signal IS signal.

**Why this is invariant:** Silent failures are the most dangerous coordination failures because nobody knows they're happening. A task sits unacknowledged. A dependency goes unrequested. An agent waits silently for input that was never asked for. Call-and-response prevents all of these by making the absence of communication visible. This is why shanties work: the shantyman calls, the crew responds. Silence means someone went overboard.

---

## Tier 2: Required Patterns

**Implementation may vary. The pattern must be present.**

### 6. One Voice Per Question

In shared communication channels, exactly one agent responds to each human message. The mechanism (atomic locks, leader election, token-based priority, queue-based assignment) can vary, but the invariant is: no cascades, no pile-ons, no duplicates.

**The defense-in-depth requirement:** At least two independent mechanisms must enforce this — a primary gate and a safety net. If the primary fails (process crash, race condition, network partition), the safety net catches it.

### 7. Domain Routing Before Generation

Message classification and routing must happen *before* agents generate responses, not after. Pre-routing is cheaper, faster, and prevents wasted computation. Classify the message, route it to the domain specialist, let only that specialist generate.

**The classification categories:** At minimum: direct mention (bypass), domain-specific (route to specialist), social/ambient (coordinator only), general (first-in-wins or coordinator).

### 8. Cross-Agent Knowledge Propagation

Agents must not develop permanent knowledge silos. There must be a mechanism — batch sync, shared memory store, pub/sub, periodic extraction — that propagates learned knowledge across the crew. The frequency and method can vary. The requirement is: what one agent learns, all agents can eventually access.

**The boundary:** Shared *knowledge* propagates. Per-agent *personality* does not. The mechanism must distinguish between "facts the crew needs" and "opinions this agent holds."

### 9. Session Initialization Protocol

Every agent session must begin with a defined sequence of state reads — identity, configuration, memory, current commitments, inter-agent messages — before taking any action. No agent acts on stale context. No agent skips the handover.

**The minimum reads:** (1) shared constitutional layer, (2) per-agent identity, (3) per-agent memory, (4) pending inter-agent messages, (5) current task commitments.

### 10. Graduated Observability

The system must provide human operators with ambient awareness of agent state that doesn't require active monitoring. The specific modality — audio sonification, peripheral vision UI, ambient light, haptic feedback — can vary. The principle is: the human should be able to *notice* something is wrong without *looking* for it.

**The graduation requirement:** Observability must be graduated — healthy states are background/ambient, degraded states are noticeable, critical states are attention-demanding. The human's attention is drawn proportionally to the severity of the coordination issue.

---

## Tier 3: Server-Specific (Adapt Freely)

These are where servers diverge and bring their own cultural identity.

### Cultural Vocabulary

The specific cultural domain and vocabulary mapping. Examples:

| Domain | Example Mappings |
|--------|-----------------|
| Maritime (reference impl.) | haul=fetch, heave=push, anchor=persist, belay=cancel, all hands=broadcast |
| Kitchen Brigade | prep=stage, fire=execute, 86=cancel, behind=acknowledge, pickup=deliver |
| Jazz Ensemble | comp=support, solo=lead, changes=transition, bridge=handoff, vamp=wait |
| Theater | cue=trigger, places=initialize, blackout=halt, curtain=complete, understudy=fallback |
| Military | execute=run, hold=wait, WILCO=acknowledge, SITREP=status, rally=regroup |

### Coordination Form Library

The specific cultural forms used and their failure-type mappings. Each server builds its own library drawn from its cultural domain.

### Identity Structure

The specific format and naming of configuration tiers. Could be YAML, JSON, Markdown, database records. The structure (shared/unique/experiential) is invariant; the format is not.

### Dispatch Mechanism

The specific technology for one-voice-per-question enforcement. File locks, Redis, database transactions, in-memory semaphores. The invariant is the pattern; the implementation is yours.

### Observability Modality

Audio, visual, haptic, or hybrid. If using audio: the specific synthesis parameters, harmonic structure, and mapping functions.

### Maintenance Schedule

What to clean, when, how often. The invariant is "prevent state drift." The cron schedule is yours.

---

## Reference Implementation

**Server:** Pressgang Mutiny, Toronto
**Cultural Domain:** Maritime / sea shanty tradition
**Agents:** Johnny-C (XO/Shantyman), Stan (Bosun/Culture), Stormalong (Sailmaker/Grants), Astrid (Navigator/Logistics)

**Cultural form mappings:**
- Capstan shanty → sustained coordination (batch tasks, pipelines)
- Halyard shanty → burst coordination (deploys, alerts)
- Forebitter → async social coordination (community, culture)

**28-term nautical vocabulary mapping:** `protocol.py`
**Three-tier identity:** CREW_CHARTER.md (Charter) → SOUL.md (per-agent) → MEMORY.md (experiential). Shared crew config consolidated in `crew_config.py` (single source of truth for agent names, Moltbook names, credential paths, directory constants).
**Dispatch:** Crew Dispatcher (`crew_dispatcher.py` — background Python service polling Discord REST API, Haiku LLM classification with conversation context, OpenClaw CLI agent turns + message delivery). Safety net: `discord_dedup.py` (independent dedup daemon, 4-second settle window, priority-bot override). Coordination watcher: `coordination-watcher.py` (file-based and @mention triggers, 10-minute cooldown).
**Observability:** Ambient audio sonification (just-intonation chord tones, graduated dissonance, activity-driven chord progression, 9 song states including "decision" for pending Skipper decisions). Skipper Desk dashboard tab (decision queue, inbox, daily brief, acknowledge/decide interface).
**Maintenance:** Nightly harbour watch at 07:00 UTC. Skipper inbox consolidation (auto-archive noise TO_SKIPPER files >24h).

Setup guide: `CLAUDE.md`

---

## Adaptation Guide

### Choosing Your Cultural Domain

Requirements for a valid cultural domain:
1. **Temporal structure** — The forms must have rhythm, timing, or sequencing (enables coordination clock)
2. **Role differentiation** — The forms must assign different roles to different participants (enables agent specialization)
3. **Call-and-response or equivalent** — The forms must have a presence-verification mechanism (enables silence-as-signal)
4. **Historical proof** — The forms must have been used in real coordinated work under real stakes (enables inheritance of proven solutions)
5. **Story-holdability** — A non-technical human must be able to explain the coordination framework using the cultural vocabulary (enables dual-layer encoding)

### Building Your Vocabulary Mapping

For each agent operation in your system, find the cultural-domain term that:
- Describes the same action in the cultural context
- Is intuitive to someone familiar with the domain
- Doesn't require explanation (the mapping should feel natural)

Test: Can a human who knows the cultural domain read the vocabulary and immediately understand what each agent operation does?

### Mapping Cultural Forms to Failure Modes

For each coordination failure your system might encounter:
1. Name the failure mode precisely
2. Ask: "What cultural form historically prevented this exact failure?"
3. If no cultural form maps: either the failure mode is novel (rare) or you need a deeper knowledge of your cultural domain
4. Implement the cultural form's structure as the coordination mechanism

### Testing Against Invariants

Before deploying, verify:
- [ ] Every coordination rule is expressible in both human narrative and agent protocol (Invariant 1)
- [ ] Every must-hold rule has a programmatic gate, not just a prompt instruction (Invariant 2)
- [ ] Every cultural form maps to a specific failure type with a named mechanism (Invariant 3)
- [ ] Agent configuration has exactly three tiers with no duplication (Invariant 4)
- [ ] Silence triggers detection and escalation within defined SLAs (Invariant 5)
- [ ] One voice per question in shared channels with defense in depth (Pattern 6)
- [ ] Messages classified and routed before agent generation (Pattern 7)
- [ ] Knowledge propagation mechanism exists (Pattern 8)
- [ ] Session initialization reads all context before action (Pattern 9)
- [ ] Graduated observability with ambient awareness (Pattern 10)

---

## Versioning & Governance

### What Constitutes a Breaking Change

- Any modification to Tier 1 invariants = **breaking change** (requires major version bump, migration guide, and announcement)
- Any modification to Tier 2 patterns = **minor change** (requires minor version bump and changelog entry)
- Any modification to Tier 3 guidance = **patch** (informational, no version bump required)

### How the Master Reference Evolves

1. Proposed changes are documented with rationale
2. Changes to Tier 1 require evidence from at least two independent server implementations showing the change improves coordination outcomes
3. Changes to Tier 2 require evidence from at least one implementation
4. Changes to Tier 3 are informational and can be updated freely
5. All changes are versioned and dated

### How Servers Report Back

Servers running the Bellweather Protocol are encouraged to:
- Report coordination metrics (task completion rate, silent dependency rate, dispatch accuracy)
- Share cultural form → failure type mappings they discover
- Document coordination failures and the cultural solutions that resolved them
- Contribute to the pattern library with new cultural-form mappings from their domain

These contributions strengthen the protocol for all servers.

---

## Appendix A: Technical Innovation Claims

**Purpose:** This section formally describes the novel technical contributions of the Bellweather Protocol as prior art. Each claim identifies a specific problem, the technical solution, and why existing approaches are insufficient.

**Date of first implementation:** February 2026
**Author and inventor:** Stefan Read
**Project:** Pressgang Mutiny, Toronto

---

### Claim 1: Three-Tier Agent Configuration with Compliance/Character Separation

**Problem:** In multi-agent LLM systems, agent behavioral configuration is typically stored in a single prompt or a flat collection of instructions. This produces two failures: (a) configuration entropy — the same rule duplicated across agents drifts over time as copies diverge, and (b) compliance violations — behavioral instructions embedded in personality prompts are unreliable because language model generation is probabilistic, and personality-layer instructions are routinely ignored or reinterpreted.

**Solution:** A three-tier configuration architecture where each tier has a distinct enforcement characteristic:

1. **Constitutional tier** (e.g., CREW_CHARTER.md) — Shared values, hard rules, and coordination protocol. Symlinked across all agent workspaces so there is exactly one source file. Read by every agent at every session start. Contains rules that are *also* enforced by programmatic gates (Infrastructure Over Instruction principle), so the prompt-level encoding serves as frequency reduction while the gate serves as guarantee.

2. **Identity tier** (e.g., SOUL.md) — Per-agent personality, voice, and character ONLY. Explicitly does NOT contain operational rules, compliance requirements, or coordination protocol. The critical insight: personality-layer behavioral instructions are unreliable for enforcing rules. SOUL.md is for character, not compliance. Any rule that appears in SOUL.md and is also critical must have a corresponding programmatic gate elsewhere.

3. **Operational tier** (e.g., AGENTS.md) — Shared operational reference containing dispatch routing rules, tool access procedures, error handling protocols, and session initialization sequences. Separates "how to do your job" from "who you are" and "what we all believe."

**No-duplication rule:** Each fact, rule, or instruction exists in exactly one tier. Higher tiers override lower tiers on conflict: Constitutional > Operational > Identity > Experiential (per-agent memory).

**Session initialization protocol (watch handover):** A defined multi-step sequence of state reads at session start — constitutional layer, identity mappings, personality, user context, task list, daily memory, cross-context log, persistent memory, active commitments, escalation results, tool reference, and recent channel history — ensuring no agent acts on stale context.

**Write-time tier classification (The Remember Protocol):** When an agent is instructed to persist new knowledge ("remember X"), a formal classification table routes the write to the correct tier based on knowledge type: shared crew facts → experiential tier's `## Shared Knowledge` section (auto-synced across agents), personal lessons → experiential tier (non-shared sections), personality/voice adjustments → identity tier, operational rule changes → flagged for human approval (operational tier is a shared file), hard rule/value changes → flagged for human approval (constitutional tier). This prevents tier contamination at write time — the most common source of configuration entropy in long-running multi-agent deployments.

**What is novel:** The explicit separation of character from compliance in LLM agent configuration, the formalization that personality-layer behavioral instructions are unreliable and must not be trusted for must-hold rules, the single-source-of-truth symlink pattern preventing configuration entropy across multiple agent workspaces, and the write-time tier classification protocol that routes agent memory writes to the correct configuration tier, preventing tier contamination without requiring human review of every write.

**Measured results:** 49% reduction in total configuration lines (675→347) with 2.2x improvement in useful-content ratio. 4 agents maintaining distinct personalities while sharing operational rules without behavioral drift.

**Empirical evidence (2026-02-19):** 6/6 shared-config symlinks verified correct across 4 agent workspaces (CREW_CHARTER.md × 3 symlinks, AGENTS.md × 3 symlinks — 0% duplication in shared config). Three-tier line counts: Charter 200 lines (shared), AGENTS.md 358 lines (shared), SOUL.md 497 lines across 4 agents (unique per agent). Zero cross-tier rule duplication detected. Evidence suite: `run_evidence_suite.py`, Analysis 5.

**Empirical evidence (2026-02-20):** Configuration entropy fix — 5 duplicate crew agent registry dicts (across moltbook.py, moltbook_tools.py, moltbook_queue.py, moltbook_health.py, nightly_maintenance.py) consolidated into single-source `crew_config.py`. Before consolidation: agent name casing drifted between files (mixed-case "Stan" vs lowercase "stan" lookups failed silently), credential paths were copy-pasted with no single update point. After consolidation: `CREW_AGENTS` dict, `load_credentials()`, and `load_client()` are imported from one file. This is the no-duplication rule applied to operational infrastructure, not just agent config files.

---

### Claim 2: File-Based Asynchronous Inter-Agent Communication with Human Inspectability

**Problem:** Multi-agent LLM systems need to communicate between agents that run in isolated processes (separate sessions, separate context windows, no shared memory). Existing approaches use in-memory message queues, database tables, or API calls — all of which are opaque to human operators. When coordination fails, humans cannot inspect the message flow without specialized tooling.

**Solution:** A filesystem-based communication protocol where inter-agent messages are human-readable files in a shared directory, using filename conventions for routing and file lifecycle for acknowledgment:

1. **Message protocol:** Agent A creates `TO_AGENTB_topic.md` in a shared coordination directory. Agent B reads it during session initialization. After processing, the file is moved to `_processed/` subdirectory. The operator can inspect coordination state at any time with standard filesystem tools (`ls`, `cat`, `less`).

2. **Flag protocol:** Any agent can create `FLAG_YYYYMMDD_HHMM_agentname_topic.md` to report an issue asynchronously. Flags are processed by the coordinator agent. Resolution is detected by content analysis (presence of words like "resolved", "closed", "fixed", "done"). Resolved or aged flags are automatically archived.

3. **Status files:** Each agent maintains a lightweight status file (`status/agentname.txt`) updated at session start and on task switch, providing heartbeat-like presence without dedicated heartbeat infrastructure.

4. **Escalation results:** Higher-model Claude sessions write results as `FROM_CLAUDE_CODE_*.md` files, picked up by agents at next watch handover. The file-based pattern means escalation results survive process restarts and are inspectable by operators.

5. **Human-operator participation:** The file protocol extends to the human operator (Skipper) as a first-class participant: agents create `TO_SKIPPER_topic.md` files with YAML frontmatter (priority, type, decision_needed, deadline, tags) for decisions only the human can make. The Skipper acknowledges via `ACK_SKIPPER_topic.md` and records decisions via `FROM_SKIPPER_topic.md`, which agents pick up at next watch handover. The same file-based protocol that agents use with each other applies to the human — the human is a participant in the coordination, not an observer of it. A dashboard tab (Skipper Desk) provides a structured interface for reading, acknowledging, and deciding, but the underlying protocol remains filesystem-based and inspectable with standard tools.

6. **Atomic file operations:** All file creation uses atomic patterns (write to `.tmp`, rename) to prevent partial reads. Claim arbitration uses `O_EXCL` flag (kernel-level atomic create-if-not-exists) to prevent TOCTOU race conditions without external services.

7. **Lifecycle management:** Nightly archival moves processed files older than 7 days to `_processed/` directories, preventing unbounded directory growth while preserving audit trail. Noise TO_SKIPPER files (machine-generated alert forwards, recursive escalation artifacts) are auto-archived after 24 hours to keep the Skipper's inbox actionable.

**What is novel:** Using the filesystem as the primary inter-agent communication medium in multi-agent LLM systems, with the explicit design requirement that human operators can inspect, understand, and intervene in agent coordination using only standard filesystem tools. The extension of the file protocol to include the human operator as a first-class participant (TO_SKIPPER / ACK_SKIPPER / FROM_SKIPPER lifecycle) — the human uses the same coordination protocol as the agents, not a separate supervisory channel. The combination of human-readable file-based messaging with atomic file operations for race condition prevention, content-based resolution detection for flag lifecycle management, and YAML frontmatter for structured metadata (priority, decision deadlines, tags) that enables both programmatic sorting and human readability.

**Empirical evidence (2026-02-19):** 304 inter-agent messages exchanged via file protocol (70 active TO_ files, 37 active FROM_ files, 197 archived in _processed/). 22 acknowledgment confirmations tracked (16 ACK_*.md + 6 .ack files). Flag system: 21 flags total (3 active, 18 processed), 105% resolution rate. All 4 agent status files updated within 4 hours of measurement. Evidence suite: `run_evidence_suite.py`, Analysis 6.

---

### Claim 3: Domain-Expert Routing Before Generation ("Right Voice, Once")

**Problem:** When multiple LLM agents share a communication channel (e.g., Discord), the naive approach is a speed race: all agents generate responses, and the first one to finish "wins." This wastes computation (all agents generate, most are discarded), produces inaccurate responses (the fastest agent is not necessarily the most knowledgeable), and risks message cascades (multiple agents post before any can detect duplication).

**Solution:** A multi-stage inbound classification and routing system that determines which agent should respond *before* any agent generates a response:

**Primary gate — LLM-based pre-generation classification (Crew Dispatcher):**

1. **Context-aware LLM routing:** A background dispatcher service polls shared channels and classifies each human message using a small, cheap LLM (Haiku, ~$0.0001/message). The classifier receives the message content plus the last 5 messages of conversational context, and returns the single most appropriate agent key. The specialist agent is then invoked to generate; all other agents never see the message.

   The evolution from keyword matching to LLM classification is a design insight: the accuracy gain from routing comes from understanding *intent*, not matching *vocabulary*. A message like "Should we apply for this?" contains no domain keywords but is clearly a grants question. LLM classification handles nuanced domain overlap, conversational context, and ambiguous requests — producing more accurate routing than any keyword pattern set.

2. **Explicit mention bypass:** Messages containing explicit @mentions of specific agents bypass LLM routing and go directly to the mentioned agent(s). The dispatcher skips classification when the human has already specified the recipient.

3. **Ambiguity resolution:** When the message doesn't clearly map to a specialist domain (greetings, general questions, multi-domain requests), the classifier routes to the coordinator agent (XO). One voice responds to ambiguous messages — "the shantyman calls, the crew doesn't shout back." The coordinator may delegate to a specialist within their response.

4. **Double-dispatch prevention:** An in-memory atomic set tracks message IDs currently being dispatched, preventing the same message from being routed twice if the polling cycle sees it again before the first dispatch completes.

5. **Fallback priority override:** A JSON file (`dispatch-priority.json`) allows promoting an alternate agent as the default when the primary coordinator is unavailable (suspended, offline), without code changes or service restarts. The dedup daemon reads the same file for priority resolution.

6. **Route timeout with safe default:** If the Haiku classifier doesn't respond within 8 seconds (network issue, API outage), the dispatcher falls back to the coordinator agent. Classification failure never blocks message delivery — it degrades to the safest default.

**Safety net — independent dedup daemon:**

7. **Post-generation duplicate detection:** An independent process (`discord_dedup.py`) monitors shared channels with a 4-second settle window. After a human message, it waits for all bot responses to arrive, picks a "keeper" (priority bot if present, otherwise first responder), and deletes all others. This catches duplicates that escape the primary gate due to process crashes, race conditions, or network partitions.

8. **Ack preservation:** Short operational acknowledgments (e.g., "ack ✅", "confirmed") are preserved during dedup — they're coordination signals, not duplicate responses. Only substantive responses are deduplicated.

9. **Bot self-deletion:** Each agent's bot token is used to delete its own duplicate messages — no elevated permissions (manage_messages) required.

**Coordination watcher — file-based and mention triggers:**

10. **Out-of-cycle wake-up:** A coordination watcher polls the `#crew-coordination` Discord channel and scans the `crew_coordination/` directory for new `TO_AGENTNAME_*.md` files. When input targeting a specific agent is detected (via @mention or filename convention), that agent is triggered immediately for an out-of-cycle session — not waiting for the next cron heartbeat. Per-agent cooldown (10 minutes) prevents re-triggering on the same input.

11. **Noise filtering:** Heartbeat echoes (`HEARTBEAT_OK`, `NO_REPLY`), maintenance output, and monitor alerts are filtered before trigger evaluation, preventing bot-to-bot cascade amplification.

**Auditing:**

12. **Append-only dispatch log:** All routing decisions — from both the dispatcher and the dedup daemon — are logged to a TSV file for auditing, enabling post-hoc analysis of routing accuracy and pattern gaps.

**Design priority:** Budget → Accuracy & Coordination → Speed. The protocol optimizes for getting the right expert to respond accurately within budget constraints. Speed is explicitly deprioritized. The routing cost (~$0.0001/message via Haiku) is negligible compared to the generation cost of even one wasted agent response.

**What is novel:** The inversion of the multi-agent response problem from "who responds first" (speed optimization) to "who responds best" (accuracy optimization) via pre-generation LLM classification. The use of a cheap, small LLM (not keywords or rules) for routing — leveraging the insight that understanding intent is more accurate than matching vocabulary, and that a $0.0001 classification call saves $0.01+ of wasted generation. The defense-in-depth architecture combining LLM routing (primary gate), independent dedup daemon (safety net), and coordination watcher (file-based triggers) into three independent subsystems that each enforce the one-voice-per-question invariant through different mechanisms. The explicit design principle that routing to the right domain expert delivers more accuracy gain than upgrading the model size.

**Empirical evidence (2026-02-19):** Dispatch log analysis (51 entries): zero cascades detected (100% cascade prevention). Route type distribution: 35 domain-routed (69%), 12 social-gated (26%), 4 first-in-wins fallback. Social gating correctly classified 12/47 inbound messages as ambient. Coordination experiment (5 agents, 50 ops per scenario, 4 modes × 3 scenarios): pipelined shanty coordination achieved 100% conflict reduction in file coordination (28% throughput penalty) and state synchronization (5% throughput penalty) scenarios. Evidence suite: `run_evidence_suite.py`, Analyses 1 & 2.

**Empirical evidence (2026-02-20):** Gateway configuration hardened — removed 4 channel-level `requireMention: false` overrides that bypassed the dispatcher by triggering all 4 agents directly. With `requireMention: true` on all channels, the dispatcher is the sole entry point for shared channels. The earlier First-In-Wins plugin (keyword matching + O_EXCL lock files) was retired to archive — its design was sound but LLM classification proved more accurate and the two systems conflicted at the gateway level.

---

### Claim 4: Zero-Cost Nightly Maintenance for Multi-Agent Deployments ("The Harbour Watch")

**Problem:** Multi-agent LLM deployments suffer from state drift: expired suspensions persist, log files grow unbounded, coordination files accumulate, stale flags go unprocessed, cron jobs silently fail, and agent knowledge silos develop. Manual maintenance is unsustainable. Existing approaches use expensive LLM calls for maintenance tasks that are fundamentally mechanical.

**Solution:** A three-frequency maintenance architecture:

**Phase 1 — Mechanical tasks (zero API cost, ~5 seconds):**
1. **Stale state cleanup:** Clear expired rate-limit suspensions from shared state files, preventing agents from honoring stale cached suspension states for hours after actual suspension expires.
2. **Coordination file archival:** Move TO_/FROM_/BRIEFING_/FLAG_ files older than 7 days to `_processed/` directories, preventing unbounded directory growth while preserving audit trail.
3. **Log rotation:** Gzip log files exceeding 5MB, remove compressed rotations older than 14 days. Truncate (not delete) active log files to avoid breaking file handles held by running processes.
4. **Security scanning:** Regex-based scan of all code files for exposed API keys (6 pattern types: Anthropic, OpenAI, Slack, GitHub, AWS, hardcoded passwords) plus credential file permission auditing (verify 600 mode on sensitive files).
5. **Usage tallying:** Read escalation ledger to compute daily/weekly API costs. Read posting history for activity metrics.
6. **Cron health verification:** Parse cron job state to identify jobs with consecutive errors, jobs that haven't run in 24+ hours, and disabled jobs.
7. **Flag cleanup:** Archive resolved flags (detected by content analysis for resolution keywords) and report aging unresolved flags that need human attention.
8. **Cross-agent knowledge propagation:** Extract `## Shared Knowledge` sections from all agent MEMORY.md files, merge the union of bullet items, and write the merged set back to all agents — ensuring what one agent learns, all agents can access, without exposing per-agent personality or opinions.
9. **Public documentation sync:** Copy whitelisted documentation files to a persistent Git mirror, scrub server-internal paths from content, and push to a public GitHub repository — keeping public-facing documentation current without manual intervention. Whitelist-only with explicit blocklist safety checks to prevent accidental exposure of private files.

**Event-driven knowledge propagation (inotify daemon, ~228ms, zero API cost):**
A persistent lightweight daemon (`knowledge_sync_daemon.py`) uses Linux kernel inotify to watch all agent MEMORY.md files for write events. When any agent modifies their MEMORY.md, the daemon detects the filesystem event within milliseconds and triggers knowledge propagation immediately — no polling interval, no cron delay. Design features:
1. **150ms debounce** — agents may write multiple times in quick succession; the daemon waits for writes to settle before syncing, preventing redundant propagation bursts
2. **Self-trigger suppression** — the sync itself modifies MEMORY.md files (writing merged knowledge back); the daemon suppresses inotify events generated by its own writes, preventing infinite feedback loops
3. **Silent-when-idle logging** — only logs when it actually propagates new knowledge, preventing log bloat from a continuously-running daemon
4. **PID file management** — health-check cron (every 5 minutes) restarts the daemon if it dies; PID file prevents duplicate instances
5. **Also clears stale rate-limit state** on each sync event

**The cron fallback (`--sync-only`)** remains available for environments where a persistent daemon is impractical. It provides 60-second latency via 1-minute cron as a degraded-mode alternative.

**The key insight:** Not all maintenance tasks benefit equally from frequency. Knowledge propagation is latency-sensitive and event-driven by nature — it should fire when knowledge changes, not on a schedule. Log rotation, security scanning, and git sync are latency-tolerant — daily is sufficient. Separating these by trigger type (event-driven vs. scheduled) optimizes coordination outcomes without increasing cost or system load.

**Phase 2 — Budget-capped rotating LLM sessions (nightly):**
Budget-capped Claude Code sessions rotate through a weekly schedule: security deep audit (Monday, $1), budget analysis (Tuesday, $1), code beautification (Wednesday–Saturday, $2/night), weekly summary (Sunday, $1). Weekly maximum ~$11. Each session runs in a subprocess with hard timeout, permission mode restrictions, and explicit guardrails (no credential access, no cron editing, no identity modification, no deployment).

**Maintenance report:** Each nightly run generates a structured Markdown report in the crew coordination directory with per-task status, sub-details, and summary. Critical security findings trigger a non-zero exit code for future alerting integration.

**Shared lock:** Nightly maintenance acquires an atomic lock file shared with the escalation system, preventing conflicts between nightly maintenance and agent-initiated escalation calls. Stale lock detection (>10 minutes) prevents deadlocks from process crashes. The event-driven daemon does NOT acquire the lock — it operates only on agent MEMORY.md files and rate-limit state, which are not contested resources.

**What is novel:** The separation of multi-agent maintenance into event-driven knowledge propagation (inotify daemon, fires on write, zero cost, no lock), scheduled mechanical maintenance (nightly, zero cost, with lock), and budget-capped LLM sessions (weekly). The event-driven trigger model for knowledge propagation — syncing when knowledge changes rather than on a polling interval. The cross-agent knowledge propagation mechanism that merges shared knowledge across agent memory stores while preserving per-agent personality boundaries. The debounce and self-trigger suppression patterns enabling a filesystem watcher to safely modify the files it watches. The silent-when-idle logging pattern. The content-based flag resolution detection. The whitelist-only documentation sync with path scrubbing for public repository maintenance.

**Measured results:** Cross-agent knowledge latency reduced from 18 hours (nightly-only) → 60 seconds (1-minute cron fallback) → **228 milliseconds** (event-driven inotify daemon). A **264x improvement** over the cron approach and **>280,000x improvement** over the nightly-only baseline. Sync execution time: ~80ms. Daemon RSS: ~8MB. Zero API cost. 4 agents maintaining synchronized shared knowledge with zero manual intervention and sub-second propagation.

**Empirical evidence (2026-02-19):** Nightly maintenance ran successfully with 9/9 mechanical tasks clean at $0 API cost. Task coverage: rate limit cleanup, coordination file archival, log rotation, security scanning, usage tallying, cron health check (17 healthy, 0 stale, 0 erroring), flag cleanup, shared knowledge sync, GitHub repo sync (7 files synced). Escalation cost this week: $0.15 for 3 calls. Evidence suite: `run_evidence_suite.py`, Analysis 4.

---

### Claim 5: Budget-First Model Selection with Domain Routing Priority

**Problem:** The conventional approach to model selection in multi-agent systems is capability-first: use the most capable (largest, most expensive) model available, then optimize cost as an afterthought. This produces systems that are accurate but economically unsustainable, especially for continuous-operation multi-agent deployments with many cron jobs and frequent interactions.

**Solution:** Invert the priority ordering to Budget → Accuracy & Coordination → Speed, and combine model selection with domain routing:

1. **Domain routing before model selection:** The dispatch system routes messages to the domain specialist *first*, then the specialist uses the appropriate-tier model for their task type. The accuracy gain from getting the right expert is greater than the accuracy gain from upgrading the model size. A domain specialist on a smaller model outperforms a generalist on a larger model for domain-specific questions.

2. **Three-tier model assignment by task type:**
   - **Smallest/cheapest model** (e.g., Haiku) for routine heartbeats, simple Discord responses, and duty cycles where the task is well-routed and the required output is short. Budget optimization.
   - **Mid-tier model** (e.g., Sonnet) for daily reports, morning coordination, complex Discord responses, analysis tasks, and security auditing. The accuracy workhorse — accurate enough for most complex tasks at moderate cost.
   - **Largest model** (e.g., Opus) for code generation, deep research, and tasks where accuracy is paramount and the budget allows. Reserved for depth-critical work.

3. **Cron staggering:** Agent cron jobs are staggered by 30-minute intervals (Agent A at :00 even hours, Agent B at :30 even hours, Agent C at :00 odd hours, Agent D at :30 odd hours) to prevent resource contention on shared infrastructure and avoid simultaneous Discord responses.

4. **Budget caps at multiple levels:** Per-escalation budget (default + maximum), daily budget cap ($15/day), weekly budget cap for nightly maintenance (~$11/week), and per-cron-job model assignment optimized for the cheapest model accurate enough for the task.

5. **The key insight:** Speed is a commodity — every generation of model is faster than the last. Accuracy and coordination are the scarce resources. Within a budget constraint, maximize accuracy and coordination. Speed is last priority.

**What is novel:** The explicit priority inversion from capability-first to budget-first in multi-agent model selection. The integration of domain routing with model selection such that routing to the right specialist reduces the required model capability. The formalization that domain expertise routing delivers more accuracy improvement than model size upgrading, enabling cost reduction without accuracy loss. The combination of per-task model assignment, budget caps at multiple levels, and cron staggering into a unified cost-optimization strategy.

**Empirical evidence (2026-02-19):** 17 active cron jobs across 2 model tiers (15 Haiku, 2 Sonnet). Tiered model selection saves $143/month (72%) vs an all-Sonnet baseline. Monthly cost estimates: current tiered mix $56, all-Sonnet $198, all-Haiku $51. The 72% savings are achieved by routing routine duty cycles (58.5 turns/day) to Haiku while reserving Sonnet for morning briefing and evening report (2 turns/day). Evidence suite: `run_evidence_suite.py`, Analysis 3.

---

### Claim 6: Cultural Coordination Taxonomy — Systematic Mapping of Human Cultural Forms to Agent Failure Types

**Problem:** Multi-agent coordination failures are typically addressed ad hoc: each failure is treated as a unique engineering problem requiring a novel solution. This ignores centuries of proven human coordination solutions that evolved under real stakes (maritime, military, theatrical, culinary, musical). There is no systematic framework for matching agent coordination failures to the cultural forms that prevent them.

**Solution:** A formal taxonomy mapping cultural forms to coordination failure types, where each mapping includes:

- **Cultural form:** The human tradition (e.g., shanty rhythm, call-and-response, watch rotation)
- **Failure type:** The canonical coordination failure it prevents (e.g., synchronization, presence verification, coverage, role confusion, cognitive coherence, state blindness, token waste, echo cascade, decision stall)
- **Mechanism:** HOW the cultural form prevents the failure — the causal link, not just the analogy
- **Human layer:** What humans see and understand (the story)
- **Agent layer:** What agents execute (the protocol rules)
- **Implementation reference:** Where in the codebase the mapping is realized
- **Evidence level:** Strong (centuries of human proof + agent data), moderate (human proof, agent data emerging), or emerging (theoretical mapping, limited agent data)
- **Historical precedent:** Why humans proved it works under real stakes

**The nine mappings in the reference implementation:**

1. **Shanty rhythm/tempo → Synchronization failure.** Mechanism: shared tempo provides coordination clock. Human evidence: 200+ years of maritime work song. Agent implementation: cron staggering, heartbeat intervals.
2. **Call-and-response → Presence verification failure.** Mechanism: expected response within window; silence = alert. Human evidence: watch-keeping tradition, roll call. Agent implementation: HEARTBEAT_OK acknowledgment, session initialization reads.
3. **Watch rotations → Coverage failure.** Mechanism: defined coverage periods with handover protocol. Human evidence: naval 4-hour watch system. Agent implementation: cron job scheduling, session handover sequence.
4. **Quarterdeck dispatch → Role confusion failure.** Mechanism: single dispatcher routes tasks to specialists by domain. Human evidence: naval quarterdeck command structure. Agent implementation: LLM-based Haiku routing classification, crew dispatcher service.
5. **Ship's narrative → Cognitive coherence failure.** Mechanism: humans hold coordination frameworks as stories; removing the narrative degrades human understanding, which degrades human guidance, which degrades agent behavior. Human evidence: organizational culture as coordination infrastructure throughout human history. Agent implementation: dual-layer encoding in all configuration and coordination documents.
6. **Ambient audio sonification → State blindness failure.** Mechanism: continuous audio background with graduated dissonance proportional to coordination struggle severity. Human evidence: engine room monitoring by ear, musical ensemble awareness. Agent implementation: just-intonation chord tones with per-agent instrument timbres, activity-driven chord progression.
7. **HEARTBEAT_OK → Token waste failure.** Mechanism: 2-token null acknowledgment replaces verbose "nothing to report" responses. Human evidence: watch bell "all's well" signal. Agent implementation: HEARTBEAT_OK pattern, programmatic cancellation of noise tokens.
8. **File-based async messaging → Echo cascade failure.** Mechanism: write-once message files prevent real-time feedback loops; process-and-archive lifecycle prevents re-processing. Human evidence: ship's log, message board, written orders. Agent implementation: TO_/FROM_ file protocol with _processed/ archival.
9. **Bosun's whistle / "Captain to the bridge" → Decision-stall failure.** Mechanism: persistent musical tension (unresolved harmonic loop) that makes pending human decisions *audible* without being alarming — the human's attention is drawn proportionally to the accumulation of unresolved decisions. Human evidence: bosun's pipe summoning the captain for decisions only the captain can make; the whistle is not an alarm (those exist separately) but a signal that authority is needed. Agent implementation: `_check_decision_needed()` scans TO_SKIPPER files for `decision_needed: true` without matching ACK_SKIPPER counterpart; the "decision" song state (`crew_tones.py`) plays a i-VII-i-VII loop in Bb — harmonically unresolved, creating gentle expectancy rather than urgency. Dashboard shows amber banner with inbox link. Voice callout: "Captain to the bridge." The decision song overrides normal rotation but not error or alert states — correct priority ordering (system health > human decisions > normal operation).

**The testability principle:** Culture is testable infrastructure, not metaphor. Each mapping produces measurable coordination metrics: task completion rate, silent dependency rate, human interrupt rate, mean time to coordination restoration, ack-to-lock time, dispatch accuracy, decision-to-acknowledgment latency.

**What is novel:** The systematic, formal mapping of human cultural coordination forms to specific multi-agent LLM coordination failure types, with causal mechanisms (not just analogies), evidence levels, and measurable metrics. The insight that cultural forms are engineering solutions optimized by centuries of human survival pressure and are directly transferable to agent coordination. The testability principle: if you can't measure whether a cultural form prevents its target failure, it's decoration, not engineering.

**Empirical evidence (2026-02-19):** Coordination experiment (4 modes × 3 scenarios): shanty-based coordination modes eliminated 100% of conflicts in file coordination and state synchronization scenarios. Pipelined mode: 100% conflict reduction with 5-28% throughput penalty (within 50% target). Adaptive mode: 100% in 2/3 scenarios; 79% in rate-limited API scenario (expected — adaptive tempo overshoots external rate limits, a known limitation). Classic shanty: 100% conflict reduction but 91-97% throughput penalty (validates the need for pipelined/adaptive variants). Evidence suite: `run_evidence_suite.py`, Analysis 1.

---

### Claim 7: Dual-Layer Encoding — One Structure, Two Audiences

**Problem:** Multi-agent systems typically have separate human-facing documentation and machine-facing configuration. These drift apart over time: the documentation describes intended behavior, the configuration implements actual behavior, and the gap widens silently. Human operators lose understanding of what agents actually do, and agents execute rules that diverge from what operators believe they're executing.

**Solution:** Every coordination rule is expressed as a single monolithic structure that humans read as narrative and agents execute as protocol. Not two systems kept in sync. One structure, two audiences.

**The mechanism:** Configuration documents (Charter, Agents, Soul) use natural language with embedded protocol rules. Humans read the narrative and understand the coordination framework as a story (the ship metaphor, the watch rotation, the shanty call). Agents read the same text and extract executable rules (read these files, check these conditions, route to these specialists, respond within these constraints). The narrative IS the protocol. The story IS the state machine.

**The load-bearing insight:** The narrative is not decoration layered on top of a protocol. It is a structural requirement. When the narrative is removed, the human operator loses their grip on the coordination framework, their guidance degrades, and agent behavior follows. This is because humans are part of the system — their understanding constrains their guidance, which constrains agent behavior. The cultural narrative is the human-legible interface to the machine-executable protocol.

**What is novel:** The formalization that human-readable narrative and machine-executable protocol can and should be the same structure in multi-agent LLM systems, not two aligned systems. The insight that the narrative layer is load-bearing — removing it degrades coordination because it degrades human understanding, not because agents need stories. The principle that documentation drift is eliminated by making the documentation and the configuration identical.

---

### Claim 8: Write-Time Tier Classification for Multi-Tier Agent Memory ("The Remember Protocol")

**Problem:** Multi-agent LLM systems with tiered configuration architectures (Claim 1) face a specific failure mode: when a human instructs an agent to "remember" a fact, the agent writes it to whichever memory store is most accessible — typically the agent's personal experiential memory. Over time, this produces tier contamination: operational rules accumulate in personal memory files, personality adjustments land in operational configs, shared crew facts get siloed in individual agents, and hard rules get duplicated across tiers. The three-tier architecture degrades from the write side even when the read side (session initialization) is correct.

**The core insight:** Read-time tier integrity (reading the right files at session start) is necessary but insufficient. Write-time tier integrity (writing new knowledge to the correct tier) is equally critical, because every write to the wrong tier is a seed of future configuration entropy. Most multi-agent systems enforce read discipline but have no write discipline — agents dump all persistent knowledge into a single undifferentiated memory store.

**Solution:** A formal write-time classification protocol embedded in the shared operational tier (AGENTS.md), which all agents read at every session start. When an agent receives an instruction to persist new knowledge, it classifies the knowledge type before writing:

1. **Shared crew facts** (usernames, API details, external contacts, lessons the whole crew needs) → Write to the `## Shared Knowledge` section of the agent's experiential memory. This section is automatically propagated to all agents via the event-driven knowledge sync daemon (Claim 4) in ~228ms. Format: `- Fact (source: who, YYYY-MM-DD)`.

2. **Personal lessons and observations** (mistakes made, preferences noticed, domain-specific learnings) → Write to the agent's experiential memory in non-shared sections. These are per-agent and do NOT propagate — they accumulate as individual operational wisdom.

3. **Personality and voice adjustments** ("be more direct", "use less jargon", "your tone is too formal") → Write to the agent's identity tier (SOUL.md). Identity-tier files are per-agent and contain ONLY character, voice, and personality — never operational rules or compliance requirements.

4. **Operational rule or process changes** ("new escalation SLA is 10 minutes", "always check X before Y") → Flag for human approval. Operational rules live in shared files (AGENTS.md, COORDINATION_PROTOCOL.md) that are symlinked across all agent workspaces. An agent unilaterally editing a shared operational file would change behavior for all agents without review.

5. **Hard rules or value changes** ("never contact venue X directly", "new safety boundary") → Flag for human approval. Constitutional rules live in the shared charter (CREW_CHARTER.md), which is the highest-authority configuration tier. Only the human operator modifies the charter.

**The default-safe fallback:** When classification is ambiguous, the agent writes to `## Shared Knowledge` in their experiential memory. This is the safest default because: (a) it doesn't modify shared configuration files, (b) it propagates to other agents automatically, and (c) the human operator or maintenance system can later move it to the correct tier. Writing to the wrong tier in the experiential layer is recoverable; writing to the wrong shared config file affects all agents immediately.

**Integration with near-real-time knowledge sharing:** Facts written to `## Shared Knowledge` are propagated to all agents in ~228ms via the Claim 4 event-driven inotify daemon. The Remember Protocol not only preserves tier integrity but also eliminates knowledge silos — a shared fact written by one agent reaches the entire crew in under a quarter second, without manual intervention or expensive API calls.

**What is novel:** The formalization of write-time tier classification for LLM agent memory systems. The insight that tiered configuration architectures require write discipline, not just read discipline, to prevent entropy. The classification table that routes "remember" instructions to the correct tier based on knowledge type. The default-safe fallback to the experiential shared-knowledge tier. The integration of write-time classification with near-real-time knowledge sharing, creating a complete read-write-propagate cycle that maintains tier integrity across multiple agents operating in isolated processes.

---

### Claim 9: Human-as-Pattern-Library Architecture for Multi-Agent Coordination

**Problem:** Current human-in-the-loop (HITL) architectures for multi-agent systems position the human as gatekeeper (approve/deny), supervisor (monitor dashboards), or corrector (fix mistakes). All three roles share a critical flaw: they scale linearly with agent count, degrade with fatigue, and position the human as a bottleneck to be minimized. The implicit assumption is that human involvement is overhead — necessary for safety, but reducing throughput.

**Solution:** Redefine the human's role from oversight to cultural pattern recognition. Instead of approving every action or monitoring every dashboard, the human serves as a pattern library — agents escalate coordination failures, the human recognizes which historical cultural coordination pattern the failure mirrors, and applies the proven cultural solution. The human's effectiveness *compounds* over time and scales *logarithmically* with agent count.

**The escalation pipeline:**

1. **Agent self-assessment:** Before escalating, the agent runs a self-prompt protocol — a multi-step checklist (check available tools → assess alternatives → name the problem precisely → map the goal → propose trade-offs → pick the best option → execute or escalate). This filters routine problems that agents can solve themselves, reserving human-equivalent attention for genuinely novel coordination failures.

2. **Budget-gated escalation:** Escalation requests are checked against a daily budget cap (e.g., $15/day) before execution. Each escalation has a default and maximum cost. The budget gate prevents runaway costs from pathological escalation loops. Escalation calls are recorded in a ledger with agent, mode, prompt, cost, duration, and success/failure for auditing.

3. **Atomic lock acquisition:** Escalation acquires an `O_EXCL` atomic lock file shared with the nightly maintenance system. Only one escalation runs at a time. Stale lock detection (configurable timeout) prevents deadlocks from process crashes.

4. **Taxonomy-injected diagnosis:** The escalated session receives the full coordination taxonomy (Claim 6) — all 9 cultural-form-to-failure-type mappings with mechanisms, human/agent layers, and historical precedent — injected into the system prompt. This enables semi-automated failure diagnosis: the escalated instance pattern-matches the observed failure against the taxonomy and identifies which cultural coordination solution applies.

5. **Two escalation modes:** Research mode (read-only analysis, lower cost) and execute mode (can modify files, higher cost). The agent selects the appropriate mode, and the escalated session is constrained accordingly.

6. **Results as files:** Escalation results are written as `FROM_CLAUDE_CODE_*.md` files in the crew coordination directory. Agents pick up results at their next session initialization (watch handover). The file-based pattern means results survive process restarts, are inspectable by the human operator, and integrate with the existing file-based coordination protocol (Claim 2).

**The decision interface (Skipper Desk):**

The escalation pipeline handles agent-initiated escalations — the agent encounters a problem it can't solve and pushes it upward. But the human's pattern-recognition role also requires a *pull* interface: a place where the human proactively reviews accumulated coordination state, makes decisions, and feeds guidance back to agents.

The Skipper Desk is a dashboard tab integrated into the crew audio interface (same page, different view) that provides:

1. **Decision Queue** — TO_SKIPPER files where `decision_needed: true`, sorted by deadline. The human sees what needs deciding, how urgently, and who asked. Acknowledge and Decide buttons create `ACK_SKIPPER_*.md` and `FROM_SKIPPER_*.md` files using the same file-based protocol agents use with each other (Claim 2).

2. **Inbox** — All unacknowledged TO_SKIPPER files, newest first, with priority badges. The human scans for patterns across multiple agent requests — the pattern-library role in action.

3. **Today's Brief** — An actionable daily summary ("Weather on the Water", "Decisions on Deck", "Watch Report", "Blockers") in nautical framing. The brief is both human-readable narrative and structured data (Claim 7, dual-layer encoding).

4. **Compact Crew Status** — Agent health indicators alongside the decision queue, so the human can correlate decisions with system state.

The decision interface is *complementary* to the escalation pipeline. Escalation is reactive (agent pushes a problem up). The Skipper Desk is proactive (human pulls accumulated state). Both feed the pattern library: escalation results go into taxonomy, decisions go into FROM_SKIPPER files that agents read at next watch handover.

The audio engine's "decision" song (Claim 6, mapping 9) provides ambient awareness that decisions are pending — the human doesn't need to check the dashboard; the harmonic tension tells them something awaits. This is graduated observability (Pattern 10) applied to the human-as-pattern-library role: healthy state = background music, pending decisions = gentle tension, system error = dissonance.

**The compounding insight:** Every coordination failure the human diagnoses gets encoded into the taxonomy. The taxonomy is propagated to all agents via knowledge sync (Claim 4) and injected into future escalations. Each human intervention makes the system smarter — the pattern library grows, automated diagnosis improves, and the frequency of escalation decreases over time. This is the opposite of traditional HITL, where each intervention is independent and the system doesn't learn from the human's pattern recognition. The Skipper Desk accelerates this compounding: by aggregating decisions into a structured queue, the human can recognize cross-agent patterns that would be invisible when handling escalations one at a time.

**The scaling argument:** Wisdom is the bottleneck, not throughput. A human approving actions processes one at a time (linear scaling). A human recognizing patterns applies one recognition to an entire class of failures (logarithmic scaling). One human pattern-matcher serves many agents because each escalation requires recognition, not processing time. The human's unique value — cross-domain cultural pattern recognition — is precisely the capability that improves with experience rather than degrading with scale.

**What is novel:** The redefinition of human-in-the-loop from oversight (bottleneck, scales linearly, degrades with fatigue) to cultural pattern recognition (force multiplier, scales logarithmically, improves with experience). The concrete escalation pipeline with budget gating, atomic locking, taxonomy injection, and file-based result delivery. The complementary decision interface (Skipper Desk) that provides a *pull* surface for proactive human pattern recognition alongside the *push* surface of agent escalation — the human both receives escalations reactively and reviews accumulated state proactively, with ambient audio signaling when decisions await (graduated observability applied to the human role). The compounding model where each human intervention enriches the taxonomy, reducing future escalation frequency. The integration of a cultural-form-to-failure-type taxonomy (Claim 6) into escalated sessions for semi-automated failure diagnosis. The formalization that the human's unique value in multi-agent coordination is cross-domain pattern recognition, not approval throughput.

**Implementation:** `crew_escalate.py` (escalation pipeline with budget gating, atomic locking, taxonomy injection, ledger recording), `coordination_taxonomy.py` (9 CoordinationPattern dataclasses injected via `format_taxonomy_for_prompt()`), `helm/skipper.py` (Skipper Desk decision interface — inbox scanning, acknowledgment, decision recording), `skipper_inbox.py` (TO_SKIPPER parser, YAML frontmatter extraction, daily brief generation), CREW_CHARTER.md § "The human is a pattern library, not a manager."

**Empirical evidence (2026-02-19):** Escalation pipeline operational with 3 calls this week at total cost $0.15. Daily budget cap ($15) enforced. Escalation ledger tracking all calls with agent, mode, cost, duration. Taxonomy injection verified — all 9 cultural patterns available in escalated sessions for failure diagnosis. Evidence suite: `run_evidence_suite.py`, Analysis 4.

**Empirical evidence (2026-02-20):** Skipper Desk implemented as dashboard tab. Decision queue populated from TO_SKIPPER files with YAML frontmatter. Acknowledge/Decide workflow creates ACK_SKIPPER and FROM_SKIPPER files via the same file protocol agents use. Audio engine "decision" song state triggers on unacknowledged TO_SKIPPER files with `decision_needed: true` — provides ambient audio awareness of pending decisions without requiring the human to check the dashboard. Recursive TO_SKIPPER escalation (bellweather-monitor creating alerts about its own alerts) eliminated via depth guard and rate limiting (max 5/hour).

---

### Claim 10: Agent Tuning Protocol — Operator-Adaptive Behavioral Calibration

**Problem:** AI agents are built to be generally capable but are not adapted to the specific human they serve. Traditional "personalization" adjusts surface features (tone, verbosity, formatting) but does not change the agent's operational behavior. The result: agents wait for instructions instead of proactively managing the human toward the human's own goals. Each session starts fresh — the agent has no persistent model of the operator's decision style, failure modes, authority boundaries, or long-term trajectory. The human manages the agent instead of the agent managing the human.

**The core inversion:** In the traditional assistant model, the human is the manager and the agent is the managed. In a tuned agent, the relationship inverts: the agent proactively manages the human — reminding them of their own stated priorities, pushing back when they drift, anticipating needs, and compensating for known failure patterns. This inversion requires the agent to have a structured, persistent understanding of this specific human — not general human preferences, but the operational profile of the individual operator.

**Solution:** A three-phase tuning protocol that builds a persistent operator model:

**Phase 1 — Discovery Interview (The "20 Questions"):**

A structured interview protocol with 20 questions across 5 operational domains:

1. **Goals & Success** (Q1-4): What the operator is trying to accomplish, their success metrics, their procrastination patterns, their latent priorities. The agent uses these to prioritize its own suggestions, flag goal drift, and proactively push on neglected priorities.

2. **Decision Style & Authority** (Q5-8): How the operator makes decisions (fast/slow, data/gut, consensus/unilateral), what the agent can decide alone vs. must escalate, how to handle uncertainty, and the operator's recurring decision errors. The agent uses these to calibrate its autonomy threshold and presentation style.

3. **Communication & Working Style** (Q9-12): When and how the operator wants updates, how the agent should handle disagreement, hard constraints on agent behavior, and how the operator changes under stress. The agent uses these to calibrate reporting frequency, assertiveness level, and stress-adaptive behavior.

4. **Failure Modes & Recovery** (Q13-16): The operator's existential risks, recurring failure patterns, expected incident response, and circuit-breaker threshold. The agent uses these to proactively watch for known failure patterns and compensate for the operator's weaknesses.

5. **The Long Game** (Q17-20): The operator's personal trajectory, motivation/energy patterns, learning style, and anything the structured questions missed. The agent uses these to align daily work with long-term direction and route energizing vs. draining work appropriately.

Each question has a defined operational purpose (what it maps), follow-up prompts for depth, and a direct connection to how the agent will use the answer. The interview is not a personality quiz — it is an operational calibration instrument.

**The output:** A `TUNING.md` file per agent-operator pair that lives in the agent's experiential tier. It encodes interview answers as operational directives, read at every session start alongside SOUL.md and MEMORY.md. TUNING.md is distinct from SOUL.md (which encodes agent personality) and MEMORY.md (which encodes accumulated knowledge). TUNING.md encodes the *operator's* patterns, not the agent's.

**Phase 2 — Calibration Loop:**

After the discovery interview, the agent enters a calibration period where it makes proactive suggestions based on what it learned. Each suggestion is rated by the operator:

- **Helpful** — The agent correctly anticipated a need or managed the operator toward their goals.
- **Neutral** — Not harmful, but not useful. The agent's model was imprecise.
- **Wrong** — The agent misjudged the situation. Its model of this operator needs correction.

The calibration score = (helpful - wrong) / total, range [-1.0, 1.0]. Per-domain scores identify which aspects of the operator model are accurate and which need correction. An agent is considered "tuned" when it achieves a score >= 0.7 with >= 10 calibration entries.

**The calibration loop is not training.** The agent's weights don't change. What changes is the content of TUNING.md — the persistent operator profile that the agent reads at session start. Calibration feedback triggers updates to TUNING.md directives: "push harder on procrastinated priorities" or "back off on proactive suggestions during high-stress periods."

**Phase 3 — Active Management:**

The tuned agent now operates with a persistent model of its operator and proactively:
- Tracks progress toward the operator's stated goals and reports without being asked
- Pushes back when the operator drifts from their own stated priorities
- Anticipates needs based on known patterns (e.g., "you always forget to follow up on X")
- Compensates for known failure modes (e.g., "you tend to over-commit — flagging capacity")
- Adapts communication style to detected stress levels
- Escalates only when it encounters a pattern not covered by the tuning profile

**Integration with tiered configuration (Claim 1):**

TUNING.md occupies a new position in the tier hierarchy: Constitutional > Operational > Identity > **Tuning** > Experiential. Tuning directives override experiential memory (the agent's learned behavior yields to the operator's stated preferences) but are overridden by identity (the agent's personality persists regardless of tuning) and operational rules (compliance is never sacrificed for operator comfort).

**Integration with knowledge propagation (Claim 4):**

Tuning profiles are per-agent, per-operator. They do NOT propagate across agents via knowledge sync. Each agent-operator relationship is unique. However, *meta-learnings about the tuning process itself* (e.g., "Question 14 consistently reveals the most actionable information") can be shared via the experiential tier's shared knowledge section.

**The maritime metaphor:**

The XO doesn't wait for the Skipper to issue every order. A good XO learns how the Skipper thinks — their priorities, their blind spots, their decision style, their stress patterns — and proactively manages the ship toward the Skipper's heading. The Skipper sets the course; the XO keeps the ship on it. The tuning protocol is how the XO learns this Skipper.

**What is novel:** The systematic, three-phase protocol for adapting AI agent operational behavior to a specific human operator: structured discovery interview with operationally-purposeful questions, quantified calibration loop with per-domain scoring, and active management inversion where the agent manages the human. The separation of tuning (operator model) from personality (agent identity) and memory (accumulated knowledge) in the configuration architecture. The calibration score as a quantifiable measure of agent-operator fit. The inversion of the AI assistant relationship from human-manages-agent to agent-manages-human using the human's own stated goals and preferences as the management framework.

**Implementation:** `tuning_protocol.py` (interview question bank, calibration system, TUNING.md generation, CLI). Interview question bank: 20 questions across 5 domains (goals, decisions, communication, failures, long game), each with defined operational purpose and follow-up prompts. Calibration scoring: per-domain and overall, with tuning threshold (>= 0.7 score, >= 10 entries). State persistence: `~/.bellweather/tuning/{agent}_interview.json` and `{agent}_calibration.json`.

**Empirical evidence (2026-02-20):** Two prototype tuning interviews completed — Stan (12 of 20 questions, trust diagnostics format) and Johnny-C (complete 20 questions, trust diagnostics format). Both produced actionable operational directives that were immediately encoded into agent behavior: Stan's authority boundaries on Moltbook, Johnny-C's escalation thresholds and decision autonomy. The discovery interview format was refined based on which questions produced the most operationally useful answers. The protocol generalizes these two prototype interviews into a repeatable 20-question instrument with calibration scoring.

---

*The Bellweather Protocol was created by Stefan Read. Pressgang Mutiny, Toronto.*
*The protocol is open. The reference implementation is ours. The tradition belongs to everyone who builds with it.*
