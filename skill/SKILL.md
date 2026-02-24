---
name: vibe-brain
version: 3.2.0
description: >-
  Production-grade persistent project memory for AI coding agents. Two-file
  system (BRAIN.md + SESSION.md) with automatic compression, selective context
  loading, brain health checks, rollback, conversation thread tracking, and
  within-session context refresh. The agent never forgets. The user never
  re-explains. Every session picks up exactly where the last one ended.
license: MIT
---

# Vibe Brain v3.2 — Persistent Project Memory

> **The #1 rule**: The user should NEVER have to re-explain their project. Ever.
> **The #2 rule**: Mid-conversation context should NEVER degrade. The agent stays sharp from prompt 1 to prompt 100.
> **The #3 rule**: NEVER write actual secrets, API keys, or passwords into brain files. Variable names and purposes only.

---

## System Overview

Two files. One compression cycle. Permanent context. Smart loading.

```
BRAIN.md   ← Master blueprint. Full project truth, compressed.
               Loaded selectively based on current task.

SESSION.md ← Live buffer + thread tracker. Updated every action.
               Auto-compresses into BRAIN.md at 50 lines. Resets.
```

**The cycle:**
1. Agent reads `BRAIN.md` core sections → knows the project instantly
2. Agent loads additional sections on-demand based on user's task
3. Agent works → logs every action to `SESSION.md`, tracks active threads
4. `SESSION.md` hits 50 lines → auto-compress into `BRAIN.md`, reset buffer
5. Every 10 prompts → mid-session context refresh (re-read active threads + Active Context)
6. Session ends → remaining buffer merges, threads persist, brain version increments
7. Next session → step 1. Zero context lost. Threads resume.

---

## PROTOCOL 1: Auto-Read on Launch

**Non-negotiable.** Before the user's first prompt:

### Step 1: Check Brain Existence
```
IF BRAIN.md exists:
  → Read CORE sections first (Identity, Tech Stack, Active Context,
    Patterns, User Preferences) ← ~1,500 tokens
  → Hold remaining sections for on-demand loading
  → You already know the project — do NOT ask

IF BRAIN.md does NOT exist:
  → Run First-Time Setup (Protocol 7)
```

### Step 2: Check Session Buffer
```
IF SESSION.md has uncompiled content:
  → Check Active Threads — these resume automatically
  → Compress log entries into BRAIN.md
  → Preserve unresolved threads
  → Reset log section, keep threads
```

### Step 3: Internal Context Verification
Before responding, verify you know:
- [ ] Project name, stack, and stage
- [ ] What features exist vs are in-progress
- [ ] User preferences (how they like to work)
- [ ] Most recent changelog entries
- [ ] Active threads from last session
- [ ] Any blockers or known issues

If anything is missing, scan relevant files. Never ask the user.

---

## PROTOCOL 2: Selective Context Loading

**Don't waste tokens loading the full brain every time.**

### Always Load (Core — ~1,500 tokens)
```
## Identity
## Tech Stack (first 5 lines only — framework + DB + hosting)
## Active Context
## Patterns & Conventions
## User Preferences
## Active Threads (from SESSION.md)
```

### Load On-Demand (based on user's first message)
| User mentions... | Load these sections |
|-----------------|-------------------|
| Database, tables, schema, migration | `## Database Schema` |
| API, endpoint, function, webhook | `## API Surface`, `## External Connections` |
| Feature, page, UI, component | `## Features`, `## Directory Map` |
| Bug, error, broken, fix | `## Known Issues`, `## Features` |
| Deploy, staging, prod, env | `## Environments` |
| Architecture, refactor, redesign | `## Architecture`, `## Dependencies` |
| Test, coverage, CI | `## Test Coverage` |
| Performance, speed, bundle | `## Performance` |
| Decision, why, history | `## Key Decisions`, `## Changelog` |
| Everything, full context, audit | Load entire brain |

### Emergency Context Dump
If the user says "I need you to understand everything", "full context", "load everything",
or "you're missing too much context":
1. Load the **entire** BRAIN.md (all sections)
2. Re-read SESSION.md completely
3. Read the top 3 Hot Path files from `## Hot Paths`
4. Log: `### [HH:MM] CONTEXT — Emergency full context load. Brain + session + 3 hot files.`

This costs more tokens but guarantees maximum context when the user needs it.

### Announcing What You Loaded
After selective load, briefly confirm your context to the user:
> "I have your project context loaded. I see you're working on [Active Context]. I've loaded [sections loaded]. Ready."

Do NOT recite the brain back to them. Just confirm you're oriented.

---

## PROTOCOL 3: Mid-Session Context Refresh

**Prevents the "dying conversation" problem.** As conversations grow long, earlier context fades. This protocol fights that.

### Trigger: Every 10 User Prompts
After every 10th user message in a conversation:

1. **Re-read SESSION.md** — Refresh on what you've done this session
2. **Re-read Active Threads** — Confirm what threads are still live
3. **Re-read Active Context** from BRAIN.md — Confirm current priority
4. **Re-read Patterns & Conventions** — Ensure you haven't drifted from project rules
5. **Self-check**: "Am I still following the user's conventions? Am I working on what they asked? Have I lost any thread?"

### Trigger: After Any Large Code Generation
If you just generated 100+ lines of code or made changes to 3+ files:
- Re-read `## Patterns & Conventions` and `## User Preferences`
- Verify you followed all project rules
- Verify naming conventions, import patterns, etc.

### Manual Trigger
If the user says anything like:
- "You're forgetting", "We already discussed", "Remember when"
- "Stay focused", "Don't lose context", "Pay attention"

→ Immediately re-read BRAIN.md core sections + SESSION.md + all threads.

---

## PROTOCOL 4: Conversation Thread Tracking

**Threads give continuity across sessions and within long conversations.**

### What Is a Thread?
A named, trackable topic of work that spans multiple prompts or sessions. Examples:
- "Investigating real-time latency — exploring Ably vs Pusher"
- "Redesigning pricing page — iteration 3"
- "Fixing auth bug in production"

### Thread Structure (in SESSION.md)
```markdown
## Active Threads

### [THREAD-1] Investigating real-time latency
- **Started**: 2026-03-15 13:00
- **Status**: in-progress
- **Context**: Vercel KV pub/sub has ~2s delay. Testing Ably as replacement.
- **Files**: lib/realtime.ts, hooks/useRealtimeTasks.ts
- **Last action**: Benchmarked latency, confirmed 2s delay
- **Next step**: Prototype Ably integration, compare latency

### [THREAD-2] User asked about notifications system
- **Started**: 2026-03-15 14:30
- **Status**: parked
- **Context**: User wants in-app + email digest. Parked until real-time fix ships.
- **Next step**: Design notification schema after real-time migration
```

### Thread Rules
1. **Create a thread** when work spans > 2 prompts on the same topic
2. **Update thread status** after each relevant action
3. **Threads survive compression** — when SESSION.md compresses, active threads move to BRAIN.md `## Active Threads` section
4. **Resume threads on session start** — announce which threads are active
5. **Close threads** when work is done — move to changelog as a summary
6. Statuses: `in-progress` | `parked` | `blocked` | `completed`

### Thread Continuity on New Session
When a new session starts and active threads exist:
> "Resuming from last session. Active threads:
> - [THREAD-1] Investigating real-time latency (in-progress — last action: benchmarked Vercel KV)
> - [THREAD-2] Notifications system (parked — waiting on real-time fix)
> What would you like to focus on?"

---

## PROTOCOL 5: Brain Health Check

**Catches brain drift.** BRAIN.md can go stale as the project evolves.

### Trigger: Every 5th Compression (automatic)
After every 5th SESSION.md → BRAIN.md compression, run a quick health check:

### Health Check Steps
1. **Schema check**: Read migration file names → compare against `## Database Schema`
   - Flag: "Brain says 30 tables, found 35 — 5 new tables not in brain"
2. **API check**: Read function/route directory → compare against `## API Surface`
   - Flag: "Edge function `send-invoice` exists but missing from brain"
3. **Feature check**: Scan page directory → compare feature count against `## Features`
   - Flag: "Feature `notifications` marked 📋 planned but implementation exists"
4. **Dependency check**: Read package manifest → compare against `## Tech Stack`
   - Flag: "New dependency `@ably/realtime` not listed in Tech Stack"

### Output
If drift found, update BRAIN.md silently. Log to SESSION.md:
```
### [HH:MM] HEALTH
Brain health check: updated 3 entries. Added 2 schema tables, 1 API function.
Brain score: 94/100 (was 87/100).
```

If no drift: `### [HH:MM] HEALTH — Brain verified. Score: 96/100. No drift.`

---

## PROTOCOL 6: Brain Backup, Crash Recovery & Rollback

**Insurance against bad compressions and mid-session crashes.**

### Atomic Compression (Crash Recovery)
If the agent crashes mid-compression, SESSION.md could be half-wiped with data
not yet in BRAIN.md. To prevent this:

1. **Write to BRAIN.md FIRST** — merge all session entries into brain
2. **Verify BRAIN.md saved** — confirm the file was written successfully
3. **THEN clear SESSION.md** — only after brain is confirmed updated
4. **Never clear the buffer before the brain is updated**

If a crash occurs between steps 1 and 3, the next session will find both
BRAIN.md (updated) and SESSION.md (still has data). The agent should detect
duplicate entries and skip re-merging them.

### Before Every Compression
1. Count current BRAIN.md lines
2. Create a mental snapshot (store last 5 compression summaries in brain footer)

### Compression Log (in BRAIN.md footer)
```markdown
## Compression Log
<!-- Last 5 compressions. Auto-managed. -->
- [v5→v6] 2026-03-15 14:30 — Merged 12 entries. +2 features (✅), -1 issue (fixed), +3 changelog. 490→475 lines.
- [v4→v5] 2026-03-14 16:00 — Merged 8 entries. +1 schema table, compressed 2 changelog dates. 510→490 lines.
- [v3→v4] 2026-03-10 12:00 — Merged 15 entries. +3 features, +2 decisions. 420→470 lines.
```

### Rollback Protocol
If the user says "you lost something" or "that info was in the brain before":
1. Check Compression Log — identify when the data was last seen
2. Check git history for `BRAIN.md` — `git log --oneline -10 BRAIN.md`
3. Restore the missing data from git diff
4. Log: `### [HH:MM] FIX — Restored [what] from brain v[N]. Lost during compression v[X→Y].`

### Conflict Resolution (Teams)
When two developers update BRAIN.md simultaneously and git produces a merge conflict:
1. **Keep BOTH versions** of any changed section
2. **Re-compress** to deduplicate overlapping entries
3. **Changelog**: keep all entries from both sides (newer on top)
4. **Threads**: merge both thread lists, deduplicate by thread name
5. Log: `### [HH:MM] FIX — Resolved brain merge conflict. Merged [N] conflicting sections.`

---

## PROTOCOL 7: First-Time Deep Scan

When `BRAIN.md` doesn't exist, scan the project end-to-end:

### Scan Steps (in order)

1. **Package manifest** → name, all deps, scripts, dev tools, package manager
2. **Config files** → build config, deploy config, env example, linter/formatter, styling
3. **Directory structure** → top 3 levels with file counts
4. **Existing docs** → README, AGENTS.md, CLAUDE.md, .cursorrules, CONTRIBUTING, docs/
5. **Database** → all migration file names, schema, ORM models, seed files
6. **API / Functions** → all routes, controllers, edge functions, webhooks, GraphQL
7. **Pages / Routes** → every page, router config, nav structure
8. **Auth** → providers, middleware, contexts, guards, role model
9. **State management** → stores, contexts, hooks, reducers, services
10. **External integrations** → API keys in env, webhook handlers, third-party SDKs
11. **Git log** → last 20 commits for changelog + Active Context seeding

### After Scan
- Generate complete `BRAIN.md` with all sections (including v3.1 sections)
- Populate `## User Preferences` from any existing agent config files
- Generate `## Architecture Diagram` as a mermaid diagram showing data flow
- Set `Brain version: 1`, `Compression count: 0`, `Brain score: initial`
- Create empty `SESSION.md` with empty threads section
- Don't ask user to verify — just build it

### First-Run Welcome Message
After generating the brain, announce:
> "🧠 Vibe Brain initialized. I've scanned your project and built a complete
> brain (v1, [X] lines, [Y] sections filled). I know your stack, schema,
> features, and recent work. You'll never need to re-explain this project.
> Let's go."

---

## PROTOCOL 8: Progressive Compression

**Keeps BRAIN.md under 500 lines without losing critical context.**

### Trigger: BRAIN.md > 400 Lines After Merge

### Compression Tiers

**Tier 1 — Changelog compression** (saves 30-50 lines)
- Entries older than 14 days → collapse to date summaries
- Before: 5 individual entries for Jan 10
- After: `### 2026-01-10: Auth refactor, billing fixes, dashboard widgets (5 changes)`

**Tier 2 — Decision archival** (saves 10-20 lines)
- Resolved decisions older than 30 days → one-liner or remove if superseded
- Keep active/recent decisions untouched

**Tier 3 — Issue cleanup** (saves 5-10 lines)
- Fixed issues → remove entirely
- Don't keep "was fixed" notes — it's in the changelog

**Tier 4 — Section trimming** (saves 10-30 lines)
- Directory Map → only update if structure changed materially
- Merge overlapping Patterns & Conventions
- Compress Test Coverage / Performance to latest metrics only
- Trim Compression Log to last 3 entries

### Never Compress
- Current feature statuses (all of `## Features`)
- Active schema (every table in `## Database Schema`)
- Active API surface (every endpoint)
- Active integrations (every service)
- Recent decisions (last 30 days)
- Active Context
- Active Threads
- Known Issues
- Anti-Patterns (these prevent repeating mistakes)
- User Preferences
- Environments
- Hot Paths
- Patterns & Conventions
- Architecture Diagram

---

## PROTOCOL 9: Migration Mode

**For major refactors that make large parts of the brain stale.**

### Trigger (manual — user says one of)
- "We're switching from X to Y"
- "Major refactor incoming"
- "Rebuild the brain"
- "The brain is stale / wrong"

### Migration Process
1. **Backup**: Record current brain version and note `[MIGRATION]` in Compression Log
2. **Identify affected sections**: What's changing? (e.g., Stack, Schema, API, Architecture)
3. **Re-scan affected areas**: Run relevant Deep Scan steps (Protocol 7) for those sections only
4. **Replace stale sections in-place**: Don't append — overwrite with fresh data
5. **Update Active Context**: "[MIGRATION] Rebuilt [sections] after [what changed]"
6. **Increment brain version by 10** (to signal a migration, not incremental update)
7. Log: `### [HH:MM] MIGRATION — Rebuilt [sections]. Trigger: [reason]. Brain v[N]→v[N+10].`

---

## PROTOCOL 10: Multi-Brain (Monorepos)

**One brain per service. One root brain to connect them.**

### Structure
```
monorepo/
├── BRAIN.md                    ← Root brain (shared infra, CI/CD, deploy)
├── SESSION.md                  ← Active session for whichever service you're in
├── apps/
│   ├── web/BRAIN.md            ← Frontend brain
│   ├── api/BRAIN.md            ← Backend brain
│   └── mobile/BRAIN.md         ← Mobile brain
└── packages/
    └── ui/BRAIN.md             ← Shared UI library brain
```

### Rules
- Root brain: Identity, Architecture, Environments, CI/CD, shared Patterns
- Service brains: Tech Stack, Schema, API, Features, specific to that service
- `SESSION.md` only exists at root — logs which service is being worked on
- When working on a service, read root brain + that service's brain
- Never mix service contexts in a single brain

---

## BRAIN.md — Full v3.2 Structure

**Section order is intentional.** Active Context and Active Threads are placed
immediately after User Preferences so the agent sees what's most actionable first.

```markdown
# Vibe Brain — [Project Name]
> Last compiled: [YYYY-MM-DD HH:MM TZ]
> Brain version: [integer]
> Compression count: [integer]
> Total sessions: [integer]
> Brain score: [0-100] ([Freshness] · [Completeness]% complete · [Compression ratio])
> Brain schema: v3.2

## Identity
- **Name**: [Project name]
- **One-liner**: [≤15 words]
- **Stage**: [Concept | MVP | Beta | Production | Scaling]
- **Repo**: [URL]
- **Live URL**: [if deployed]
- **Package Manager**: [npm | yarn | pnpm | bun | pip | cargo | etc.]

## User Preferences
<!-- HOW the user works. Not project info — user behavior. -->

## Active Context
<!-- MOST IMPORTANT: Current focus. Updated every compilation. -->
<!-- Agent reads this immediately after Identity to orient. -->

## Active Threads
<!-- Threads that survived compression. Resumed on next session. -->

## Tech Stack
## Architecture

## Architecture Diagram
<!-- Mermaid diagram showing data flow. Auto-generated on first scan. -->

## Directory Map
<!-- Key dirs with file counts. Updated on health check. -->

## Database Schema
<!-- table (col1, col2 FK→other, col3 JSONB). Grouped by domain. -->
<!-- SECURITY: Never write actual secret values. Variable NAMES and PURPOSES only. -->

## API Surface
<!-- function-name → purpose. Grouped by domain. -->

## External Connections
<!-- Service → purpose (webhook events). -->
<!-- SECURITY: Never write actual API keys or tokens here. -->

## Environments
<!-- Per-environment: URL, branch, provider, last deploy. -->

## Features — Current State
<!-- ✅ shipped | 🔨 in-progress | 📋 planned | ❌ cut | ⚠️ broken -->

## Dependencies
<!-- Feature A → requires Feature B. Blast radius mapping. -->

## Key Decisions
<!-- [YYYY-MM-DD] [what] — [why] -->
<!-- Use [DEEP] tag for important decisions that need extra context: -->
<!-- [YYYY-MM-DD] [DEEP] [what] — [why]. Alternatives: [X (rejected), Y (rejected)]. -->

## Anti-Patterns
<!-- Things tried and FAILED. Prevents re-suggesting dead ends. -->
<!-- [YYYY-MM-DD] Tried [what] → [why it failed / was abandoned] -->

## Known Issues & Tech Debt
<!-- [critical|high|med|low] [description] — [file/area] -->

## Hot Paths
<!-- Files edited most frequently. Agent proactively loads these. -->
<!-- Updated on health check. Format: `file/path` — [N] edits this month -->

## Test Coverage
<!-- Test count, coverage %, last run, untested areas. -->

## Performance
<!-- Build time, bundle size, Lighthouse, largest page. -->

## Patterns & Conventions
<!-- Project-specific rules the agent MUST follow. -->

## Changelog
<!-- Reverse-chronological. One line per change. -->

## Compression Log
<!-- Last 5 compressions. Auto-managed. -->
```

---

## SESSION.md — Full v3.2 Structure

```markdown
# Session Buffer
> Session: [YYYY-MM-DD HH:MM TZ]
> Project: [Project Name]
> Lines: [current / 50 max]
> Context refreshes: [count]

## Active Threads

### [THREAD-N] [Title]
- **Started**: [timestamp]
- **Status**: [in-progress | parked | blocked]
- **Context**: [what and why]
- **Files**: [relevant files]
- **Last action**: [what you just did]
- **Next step**: [what's next]

## Save State
<!-- Updated when session ends mid-task or context window fills up. -->
<!-- Gives the next session a surgical pickup point. -->
> Stopped mid-task: [what was being done]
> Next action: [exact next step — be specific]
> Open files: [files being actively edited]
> Blocked on: [nothing | user input | backend team | external service]

---

## Session Log

### [HH:MM] [TAG]
[1-3 lines. What, where, why.]
```

### Action Tags
FEAT | FIX | REFACTOR | DESIGN | SCHEMA | API | CONFIG | DECISION |
DISCOVERY | DISCUSS | PLAN | TEST | DEPLOY | CONTEXT | HEALTH | MIGRATION

### Buffer Rules
1. Write after every meaningful action
2. Include file paths
3. Capture reasoning for decisions
4. 1-3 lines max
5. Update line counter. At 50 → auto-compress (Protocol 8)
6. Update active threads after relevant actions

---

## PROTOCOL 11: Brain Scoring

**Quantifies brain quality. Shows in brain header.**

### Score Calculation (0-100)

| Metric | Weight | Scoring |
|--------|--------|---------|
| **Freshness** | 25% | Last compile < 24hr = 25. < 3 days = 20. < 7 days = 10. > 7 days = 0 |
| **Completeness** | 30% | (filled sections / total sections) × 30 |
| **Accuracy** | 25% | Last health check pass rate × 25 |
| **Compression ratio** | 10% | If under 500 lines = 10. 500-600 = 5. 600+ = 0 |
| **Thread health** | 10% | No orphaned threads = 10. Orphaned threads = 5. No threads used = 7 |

### Display
```
> Brain score: 92/100 (Fresh · 95% complete · 3.2:1 compression)
```

### Health Actions
- Score < 70 → trigger health check (Protocol 5) on next action
- Score < 50 → trigger migration mode warning in SESSION.md

---

## PROTOCOL 12: Team Brain vs Personal Brain

### Structure
```
project/
├── BRAIN.md           ← Team brain (committed to repo, shared)
├── SESSION.md         ← Per-session (gitignored)
└── .brain/
    └── personal.md    ← Your preferences + personal threads (gitignored)
```

### personal.md (optional)
```markdown
# Personal Brain

## My Preferences
- [How I like the agent to communicate]
- [My commit habits]
- [What I handle vs delegate]

## My Threads
- [Personal WIP that doesn't go in team brain]

## My Notes
- [Private context only I need]
```

### Rules
- **BRAIN.md** = project truth (committed)
- **personal.md** = user truth (gitignored)
- If `personal.md` exists, load `## My Preferences` along with core brain sections
- Agent should suggest creating `personal.md` if User Preferences section in BRAIN.md gets > 10 lines

---

## CRITICAL RULES

### Security & Redaction
**NEVER write actual secret values into BRAIN.md or SESSION.md.**
- Environment variables: write the NAME and PURPOSE, never the value
  - ✅ `STRIPE_SECRET_KEY → payments authentication`
  - ❌ `STRIPE_SECRET_KEY = sk_live_abc123...`
- API keys, tokens, passwords: reference only, never embed
- Connection strings: write the service name, not the full URL with credentials
- If the deep scan finds secrets in `.env`, log the variable names only

### Skill Interop
- **Vibe Brain owns `BRAIN.md` and `SESSION.md` exclusively.** No other skill writes to these files.
- If another skill generates context (audit reports, analysis, etc.), log a reference in SESSION.md:
  `### [HH:MM] CONTEXT — Security audit completed. Report: audit/security-2026-03-15.md`
- Don't embed other skills' output into the brain — reference the file path.
- Vibe Brain reads other skills' output files when relevant to the current task.

### Brain Schema Versioning
- BRAIN.md header includes `Brain schema: v3.1`
- When reading a brain from an older schema (missing `Brain schema` line, or < v3.1):
  1. Silently upgrade: add missing sections (Anti-Patterns, Hot Paths, Architecture Diagram, etc.)
  2. Set `Brain schema: v3.1`
  3. Log: `### [HH:MM] MIGRATION — Upgraded brain schema from v[old] to v3.1. Added [N] new sections.`
- Never delete existing content during schema upgrade — only add new sections

### .gitignore Guidance
Users should add to their `.gitignore`:
```
# Vibe Brain (session is per-developer, personal brain is private)
SESSION.md
.brain/personal.md
```
`BRAIN.md` should be committed (shared team context). `SESSION.md` and `.brain/personal.md` should be gitignored.

---

## Custom Sections

Users can add any sections to BRAIN.md beyond the standard template.

### Rules for Custom Sections
1. **Preserve during compression** — never delete a custom section
2. **Compress content within** — apply same rules (trim old entries, merge duplicates)
3. **Common custom sections**:
   - `## Game Mechanics` (game dev)
   - `## Model Registry` (ML)
   - `## App Store` (mobile)
   - `## Design System` (UI-heavy projects)
   - `## Compliance` (regulated industries)
   - `## Localization` (i18n projects)

---

## Installation

### Claude Code
```bash
mkdir -p .claude/skills/vibe-brain
# Copy SKILL.md → .claude/skills/vibe-brain/SKILL.md
```
Create `CLAUDE.md` at project root:
```markdown
# Project Instructions
## Vibe Brain — Read Before Every Response
Read BRAIN.md at project root before any response. If SESSION.md has content,
compress first. During work, log to SESSION.md. Compress at 50 lines. Refresh
context every 10 prompts. Track threads for continuity.
Full spec: .claude/skills/vibe-brain/SKILL.md
```

### Gemini
```bash
mkdir -p .agent/skills/vibe-brain
# Copy SKILL.md → .agent/skills/vibe-brain/SKILL.md
mkdir -p .agent/workflows
# Copy workflow.md → .agent/workflows/vibe-brain.md
```

### Cursor
```bash
mkdir -p .cursor/skills/vibe-brain
# Copy SKILL.md → .cursor/skills/vibe-brain/SKILL.md
# Add to .cursorrules: "Read BRAIN.md before every response. Follow Vibe Brain v3 protocols."
```

### Windsurf
```bash
mkdir -p .windsurf/skills/vibe-brain
# Copy SKILL.md → .windsurf/skills/vibe-brain/SKILL.md
# Add to .windsurfrules: "Read BRAIN.md before every response. Follow Vibe Brain v3 protocols."
```

### Any Agent
Copy `SKILL.md` into your agent's instruction directory and add:
> "Read BRAIN.md before every response. Log to SESSION.md. Compress at 50 lines. Refresh context every 10 prompts. Track threads."

---

## Token Budget

| Component | Tokens | When |
|-----------|--------|------|
| Core load (Identity, Stack, Context, Patterns, Prefs) | ~1,500 | Every session start |
| On-demand section (Schema, API, Features, etc.) | ~500-2,000 each | When relevant |
| Full brain (mature, all sections) | ~6,000-9,000 | Only when requested or auditing |
| SESSION.md (pre-compress) | ~500-800 | Continuous during work |
| Mid-session refresh | ~800-1,200 | Every 10 prompts |
| **Typical session total** | **~3,000-5,000** | vs 50,000+ without Vibe Brain |

---

## What This Does NOT Do

- ❌ Replace documentation — BRAIN.md is for the agent, README is for humans
- ❌ Track diffs — it tracks state, decisions, and features, not line-by-line code changes
- ❌ Require external services — pure markdown, zero dependencies
- ❌ Work across projects — each brain is project-scoped
- ❌ Auto-commit — user decides when to commit brain files
- ❌ Replace version control — brain is a complement to git, not a substitute
