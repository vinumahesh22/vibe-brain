---
description: Persistent project memory. Auto-activates every session. Read BRAIN.md before first response. Refresh context every 10 prompts.
---

# Vibe Brain v3.2 Workflow

**Fully automatic.** The agent follows this without being asked.

## On Every New Conversation (BEFORE First Response)

1. **Read `BRAIN.md` core sections**: Identity, User Preferences, Active Context, Active Threads, Patterns
   - If brain exists → you have project context. Do NOT ask the user what their project is.
   - If no brain → run First-Time Deep Scan (Protocol 7 in SKILL.md)

2. **Check `SESSION.md`** for leftover content
   - If active threads exist → note them for resumption
   - If Save State exists → read it for exact pickup point
   - If log entries exist → compress into BRAIN.md, preserve threads, reset log
   - If empty → create fresh SESSION.md

3. **Resume threads**: Briefly tell the user what threads are active from last session

4. **Load on-demand sections** based on user's first message (Protocol 2 in SKILL.md)
   - If user says "full context" / "load everything" → Emergency Context Dump (entire brain + session + hot files)

## During Active Work

5. **Log every action** to SESSION.md with timestamp + tag
   - Tags: FEAT | FIX | REFACTOR | DESIGN | SCHEMA | API | CONFIG | DECISION | DISCOVERY | DISCUSS | PLAN | TEST | DEPLOY | CONTEXT | HEALTH | MIGRATION
   - 1-3 lines per entry. Include file paths. Update line counter.

6. **Track threads**: Create threads for multi-prompt work. Update status after each action.

7. **At 50 lines** → auto-compress into BRAIN.md, reset log, preserve active threads. No asking.

8. **Every 10 user prompts** → mid-session context refresh (Protocol 3). Re-read SESSION.md, threads, Active Context, Patterns.

9. **After large code generation** (100+ lines or 3+ files) → re-read Patterns & User Preferences.

10. **Every 5th compression** → brain health check (Protocol 5). Verify schema, API, features match reality.

## On Session End

11. Compile remaining SESSION.md entries into BRAIN.md
12. Move completed threads to changelog. Keep active/parked threads in BRAIN.md `## Active Threads`
13. **Write Save State** if stopping mid-task (what was being done, next action, open files, blockers)
14. Update Active Context, brain score, and increment Total sessions counter
15. Reset SESSION.md (keep Save State if mid-task)

## Key Rules

- **BRAIN.md stays under 500 lines.** Progressive compression triggers at 400.
- **Selective loading.** Don't waste tokens loading sections irrelevant to current task.
- **Threads survive sessions.** Active threads carry over via BRAIN.md.
- **Save State captures exact position.** Next session picks up surgically.
- **Context refreshes every 10 prompts.** Prevents context degradation in long conversations.
- **Health checks every 5th compression.** Catches brain drift automatically.
- **Never write secrets.** Variable names and purposes only.
- **Never ask the user to re-explain their project.** You have the brain. Use it.
