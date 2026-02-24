# 🧠 Vibe Brain v3.2

**Your AI coding agent never forgets. Not between sessions. Not mid-conversation.**

Persistent project memory that gives your AI assistant full context about your codebase — architecture, features, APIs, database schema, decisions, active threads, and exactly where you left off — on every single conversation. Smart loading saves tokens. Mid-session refresh prevents context decay. Thread tracking gives continuity across sessions.

Works with **Claude Code** · **Gemini** · **Cursor** · **Windsurf** · **Any AI coding agent**

---

## The Problem

Every new conversation:
> *"This is a Next.js app with Prisma..."*
> *"We use tRPC, not REST..."*
> *"We already built auth last week..."*

And mid-conversation (prompt 15+):
> *"You're forgetting what I said earlier..."*
> *"We already decided on Ably, not Pusher..."*
> *"Stay focused on what I asked..."*

You waste tokens re-syncing. Past decisions get forgotten. Features get re-proposed. Context degrades after ~10 prompts. You go in circles.

**Vibe Brain fixes all of this. Permanently.**

---

## How It Works

```
┌───────────────────────────────────────────────────────────────┐
│                       YOUR PROJECT                            │
│                                                               │
│  BRAIN.md ← Master blueprint. Smart-loaded.                  │
│  ↑          Core sections on start. Rest on-demand.           │
│  │                                                            │
│  │ auto-compress         ← every 50 lines                    │
│  │ health check          ← every 5th compression              │
│  │ context refresh       ← every 10 prompts                  │
│  │                                                            │
│  SESSION.md ← Live buffer + active threads.                   │
│               Logs actions. Tracks work topics.               │
│               Resets after compression.                        │
│               Threads survive.                                │
└───────────────────────────────────────────────────────────────┘
```

### The Full Cycle

1. **Session starts** → Agent loads BRAIN.md core (~1,500 tokens) + resumes threads
2. **User sends first message** → Agent loads relevant sections on-demand
3. **During work** → Actions logged to SESSION.md, threads tracked
4. **Every 10 prompts** → mid-session context refresh (re-reads key context)
5. **SESSION.md hits 50 lines** → auto-compress into BRAIN.md, reset, threads survive
6. **Every 5th compression** → brain health check (verifies brain matches reality)
7. **Session ends** → remaining buffer merges, threads persist in brain
8. **Next session** → step 1. Zero context lost. Threads resume.

---

## 12 Core Protocols

| # | Protocol | What It Does |
|---|----------|-------------|
| 1 | **Auto-Read** | Reads brain before your first prompt. You never re-explain. |
| 2 | **Selective Loading** | Only loads relevant brain sections. Saves 50-70% tokens. Emergency dump available. |
| 3 | **Mid-Session Refresh** | Re-reads key context every 10 prompts. Prevents context decay. |
| 4 | **Thread Tracking** | Named work topics that survive sessions. Resume where you left off. |
| 5 | **Brain Health Check** | Every 5th compression, verifies brain matches actual project state. |
| 6 | **Backup, Crash Recovery & Rollback** | Atomic compression, compression log, git recovery if info gets lost. |
| 7 | **Deep Scan** | 11-step project scan generates brain from scratch. No manual setup. |
| 8 | **Progressive Compression** | Brain stays under 500 lines. Old entries collapse. Nothing lost. |
| 9 | **Migration Mode** | Major refactor? Agent re-scans affected sections, rebuilds in-place. |
| 10 | **Multi-Brain** | Monorepo? One brain per service + root brain to connect them. |
| 11 | **Brain Scoring** | Quality score (0-100) based on freshness, completeness, accuracy. |
| 12 | **Team + Personal** | Shared team brain (committed) + personal preferences (gitignored). |

### Critical Rules (Always Enforced)
| Rule | What It Does |
|------|-------------|
| **Security & Redaction** | Never writes secrets, API keys, or passwords into brain files. Variable names only. |
| **Crash Recovery** | Atomic compression: brain updated FIRST, then session cleared. No data loss on crash. |
| **Conflict Resolution** | Teams: merge conflicts resolved by keeping both sides + deduplicating. |
| **Skill Interop** | Vibe Brain owns BRAIN.md/SESSION.md exclusively. Other skills log references only. |
| **Schema Versioning** | Old brains auto-upgrade to latest schema format. No manual migration needed. |

---

## What Goes in BRAIN.md?

| Section | What It Stores |
|---------|---------------|
| **Identity** | Project name, stage, repo, URL |
| **User Preferences** | How YOU work (comm style, commit habits, delegation, design standards) |
| **Active Context** | What you're currently working on *(moved near top for immediate orientation)* |
| **Active Threads** | Multi-session work topics with status *(moved near top)* |
| **Tech Stack** | Every framework, service, key library |
| **Architecture** | Data flow, auth model, tenancy, real-time |
| **Architecture Diagram** | Mermaid diagram showing data flow (auto-generated) |
| **Directory Map** | Key directories with file counts |
| **Database Schema** | Every table with columns and relationships |
| **API Surface** | Every endpoint grouped by domain |
| **External Connections** | Every third-party integration |
| **Environments** | Dev, staging, prod — URLs, branches, last deploy |
| **Features** | Every feature with status (✅ 🔨 📋 ❌ ⚠️) |
| **Dependencies** | Feature dependency graph (blast radius) |
| **Key Decisions** | Architecture/product decisions with reasoning ([DEEP] tag for major ones) |
| **Anti-Patterns** | Things tried and failed — prevents re-suggesting dead ends |
| **Known Issues** | Active bugs and tech debt |
| **Hot Paths** | Most frequently edited files — agent proactively loads context |
| **Test Coverage** | Test count, coverage %, untested areas |
| **Performance** | Build time, bundle size, Lighthouse scores |
| **Patterns & Conventions** | Project-specific rules the agent must follow |
| **Changelog** | Timestamped history |
| **Compression Log** | Last 5 compressions for rollback |

---

## Quick Start

### Claude Code
```bash
mkdir -p .claude/skills/vibe-brain
cp skill/SKILL.md .claude/skills/vibe-brain/SKILL.md

cat > CLAUDE.md << 'EOF'
# Project Instructions
## Vibe Brain — Read Before Every Response
Read BRAIN.md core sections before any response. Load other sections on-demand.
If SESSION.md has content, compress first. Log to SESSION.md during work.
Compress at 50 lines. Refresh context every 10 prompts. Track threads.
Full spec: .claude/skills/vibe-brain/SKILL.md
EOF
```

### Gemini
```bash
mkdir -p .agent/skills/vibe-brain
cp skill/SKILL.md .agent/skills/vibe-brain/SKILL.md
mkdir -p .agent/workflows
cp skill/workflow.md .agent/workflows/vibe-brain.md
```

### Cursor
```bash
mkdir -p .cursor/skills/vibe-brain
cp skill/SKILL.md .cursor/skills/vibe-brain/SKILL.md
echo 'Read BRAIN.md before every response. Follow Vibe Brain v3.2 protocols in .cursor/skills/vibe-brain/SKILL.md' >> .cursorrules
```

### Windsurf
```bash
mkdir -p .windsurf/skills/vibe-brain
cp skill/SKILL.md .windsurf/skills/vibe-brain/SKILL.md
echo 'Read BRAIN.md before every response. Follow Vibe Brain v3.2 protocols in .windsurf/skills/vibe-brain/SKILL.md' >> .windsurfrules
```

### Any Agent
Copy `skill/SKILL.md` → agent instructions. Add: *"Follow Vibe Brain v3.2 protocols."*

---

## Repo Structure

```
vibe-brain/
├── README.md
├── LICENSE (MIT)
├── CONTRIBUTING.md
├── CHANGELOG.md
├── .gitignore
│
├── skill/
│   ├── SKILL.md             ← Full v3.2 spec (12 protocols + 5 critical rules)
│   └── workflow.md           ← Auto-activation workflow
│
├── templates/
│   ├── BRAIN.md              ← Guided template (every section commented)
│   └── SESSION.md            ← Empty buffer + threads
│
├── examples/
│   ├── BRAIN-example.md      ← Full populated brain (TaskFlow SaaS)
│   └── SESSION-example.md    ← Active session with threads
│
└── .github/
    ├── workflows/
    │   └── brain-check.yml   ← CI: warn if brain is stale
    ├── ISSUE_TEMPLATE/
    └── PULL_REQUEST_TEMPLATE.md
```

---

## Token Savings

| Without Vibe Brain | With Vibe Brain v3.2 |
|-------------------|-------------------|
| 50,000+ tokens re-explaining | ~3,000-5,000 total (selective loading) |
| Context dies at prompt ~15 | Refreshes every 10 prompts |
| Forgotten decisions | Tracked in brain with timestamps + [DEEP] tags |
| Lost threads between sessions | Threads resume automatically + Save State for exact pickup |
| Brain drift (stale info) | Health checks every 5th compression |
| Repeating failed approaches | Anti-Patterns section prevents dead ends |
| Secrets leaked to context | Security protocol: variable names only |
| Data loss on crash | Atomic compression: brain first, then clear |

---

## FAQ

**How big does BRAIN.md get?** — Hard cap at 500 lines. Progressive compression.

**What if the brain is wrong?** — Edit directly. Agent reads corrections next session. Or say "rebuild the brain" for Migration Mode.

**Multiple agents?** — Yes, all read same BRAIN.md. Avoid simultaneous SESSION.md writes.

**Non-code projects?** — Adapt sections to your domain. Custom sections are preserved during compression.

**My stack isn't covered?** — Stack-agnostic. Works with any language, framework, or architecture.

**Monorepo?** — Multi-brain protocol: one brain per service, root brain connects them.

**Mid-conversation forgetting?** — Context refresh every 10 prompts + thread tracking prevents this.

**What about secrets?** — Vibe Brain never writes actual API keys or passwords. Variable names and purposes only.

**What if the agent crashes mid-compression?** — Atomic compression: brain is updated first, session is cleared after. No data loss.

**What gets committed?** — BRAIN.md (shared). SESSION.md and .brain/personal.md should be gitignored.

---

## Badge

Add to your project README:
```markdown
[![Vibe Brain](https://img.shields.io/badge/🧠_Vibe_Brain_v3.2-enabled-blue)](https://github.com/m3swizz/vibe-brain)
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
