# The Bellweather Protocol — Core Reference

**Version:** 1.1
**Author:** Stefan Read
**Project:** Stefan Read
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
**Five-tier identity:** CREW_CHARTER.md (Constitutional) → AGENTS.md (Operational) → SOUL.md (Identity) → TUNING.md (Tuning, per-operator) → MEMORY.md (Experiential). Override order is context-dependent: individual mode elevates Tuning above Identity; organizational mode preserves Identity above Tuning. Tuning tier deployed — all 4 agents at 20/20 interviews, TUNING.md generated and deployed to each workspace (`tuning_protocol.py`). Shared crew config consolidated in `crew_config.py` (single source of truth for agent names, Moltbook names, credential paths, directory constants).
**Dispatch:** Crew Dispatcher (`crew_dispatcher.py` — background Python service polling Discord REST API, Haiku LLM classification with conversation context, OpenClaw CLI agent turns + message delivery). Safety net: `/root/.openclaw/discord_dedup.py` (independent dedup daemon, 4-second settle window, priority-bot override). Coordination watcher: `/root/.openclaw/coordination-watcher.py` (file-based and @mention triggers, 10-minute cooldown).
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
- [ ] Agent configuration has exactly five tiers with no duplication (Invariant 4)
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

---

*The Bellweather Protocol was created by Stefan Read.*
*The protocol is open. The reference implementation is ours. The tradition belongs to everyone who builds with it.*
