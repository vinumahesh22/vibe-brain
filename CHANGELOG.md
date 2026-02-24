# Changelog

All notable changes documented here. Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [3.2.1] - 2026-02-24

### Added
- **Cross-Platform Auto-Read Templates** — Pre-built config files for every major AI coding platform. Drop into your project root so the agent reads `BRAIN.md` automatically on every new conversation.
  - `templates/CLAUDE.md` → Claude Code
  - `templates/.cursorrules` → Cursor
  - `templates/.windsurfrules` → Windsurf
  - `templates/.clinerules` → Cline
  - `templates/copilot-instructions.md` → GitHub Copilot
- **Expanded platform support** — README now lists Cline, GitHub Copilot, and Antigravity alongside existing platforms
- **Quick Start simplified** — All platform setups now use `cp` from templates instead of inline `echo` or `cat` commands

## [3.2.0] - 2026-02-24

### Added
- **Smart Section Ordering** — Active Context and Active Threads moved directly after User Preferences. Agent orients immediately on what matters most.
- **Emergency Context Dump** — User says "full context" → agent loads entire brain + session + top 3 hot path files. Maximum context when needed.
- **Session Handoff (Save State)** — SESSION.md now includes a Save State section capturing exact mid-task position, next action, open files, and blockers. Surgical session pickup.
- **Usage Stats** — Brain header now includes `Total sessions` counter. Shows brain maturity at a glance.

### Changed
- BRAIN.md section order: Identity → User Preferences → Active Context → Active Threads → Tech Stack → ...
- Brain schema bumped to v3.2
- SESSION.md structure updated with Save State block
- BRAIN.md now has 23 sections in optimized order

## [3.1.0] - 2026-02-24

### Added
- **Security & Redaction** — Rule #3: Never write actual secrets, API keys, or passwords into brain files. Variable names and purposes only.
- **Atomic Crash Recovery** — Compression is now atomic: BRAIN.md updated first, then SESSION.md cleared. No data loss on crash.
- **Conflict Resolution** — Team guidance: merge conflicts resolved by keeping both sides + deduplicating.
- **Anti-Patterns section** — Things tried and failed. Prevents agent from re-suggesting dead ends.
- **Hot Paths section** — Most frequently edited files. Agent proactively loads context for these.
- **Architecture Diagram section** — Mermaid diagram of data flow, auto-generated on first scan.
- **[DEEP] Decision Tags** — Major decisions include alternatives considered and reasoning for each.
- **Brain Schema Versioning** — `Brain schema: v3.1` header. Old brains auto-upgrade on next read.
- **Skill Interop** — Vibe Brain owns BRAIN.md/SESSION.md exclusively. Other skills log references only.
- **.gitignore Guidance** — SESSION.md and .brain/personal.md should be gitignored.
- **First-Run Welcome Message** — Agent announces brain initialization with line count and sections filled.

### Changed
- Protocol 6 renamed to "Backup, Crash Recovery & Rollback" with atomic compression steps
- Never-compress list expanded: Anti-Patterns, Hot Paths, Architecture Diagram
- BRAIN.md structure now includes 23 sections (was 20)
- Security comments added to Database Schema and External Connections sections in templates
- Key Decisions section now documents [DEEP] tag usage

## [3.0.0] - 2026-02-24

### Added
- **Selective Context Loading** (Protocol 2) — Load only relevant brain sections. Saves 50-70% tokens.
- **Mid-Session Context Refresh** (Protocol 3) — Re-reads key context every 10 prompts. Prevents context decay.
- **Conversation Thread Tracking** (Protocol 4) — Named work topics that survive sessions and compressions.
- **Brain Health Check** (Protocol 5) — Every 5th compression, verifies brain matches reality (schema, API, features).
- **Brain Backup & Rollback** (Protocol 6) — Compression Log tracks last 5 compressions. Git recovery for lost info.
- **Migration Mode** (Protocol 9) — Major refactor triggers targeted re-scan of affected sections.
- **Multi-Brain** (Protocol 10) — Monorepo support: one brain per service + root brain.
- **Brain Scoring** (Protocol 11) — Quality score 0-100 (freshness, completeness, accuracy, compression ratio).
- **Team + Personal Brain** (Protocol 12) — Shared team brain (committed) + personal.md (gitignored).
- **User Preferences** section — How the user works (comm style, habits, delegation rules)
- **Environments** section — Dev/staging/prod URLs, branches, last deploy
- **Dependencies** section — Feature dependency graph (blast radius mapping)
- **Test Coverage** section — Test count, coverage %, untested areas
- **Performance** section — Build time, bundle size, Lighthouse scores
- **Active Threads** section — Persistent threads from SESSION.md
- **Compression Log** section — Last 5 compression summaries for rollback
- **Custom section preservation** — User-defined sections survive all compressions
- New action tags: `HEALTH`, `MIGRATION`
- Context refresh counter in SESSION.md header
- GitHub Action for CI brain staleness checks
- Badge for project READMEs

### Changed
- SKILL.md restructured into 12 numbered protocols
- SESSION.md now has separate Threads and Log sections
- Brain header now includes brain score
- Workflow includes thread resumption, selective loading, refresh triggers
- README documents all 12 protocols with token savings data
- Examples show threads, new sections, and brain scoring in action

## [2.0.0] - 2026-02-24

### Added
- Compression Engine (50-line SESSION.md threshold)
- Auto-Read on Launch (BRAIN.md before first prompt)
- Progressive Compression (500-line brain cap)
- Active Context section
- Patterns & Conventions section
- Directory Map section
- Brain versioning + compression count
- Line counter in SESSION.md
- Context verification step
- 11-step deep scan protocol
- Claude Code native support
- Tags: PLAN, TEST, DEPLOY, CONTEXT

## [1.0.0] - 2026-02-24

### Added
- Initial release: BRAIN.md + SESSION.md two-file system
- SKILL.md with compilation process
- Templates, examples, MIT license
