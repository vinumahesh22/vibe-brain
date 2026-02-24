# 📊 Vibe Brain — Token Savings & Analytics

> **Bottom line: ~60% fewer tokens per session. Every protocol pays for itself multiple times over.**

---

## The Problem Vibe Brain Solves

AI coding agents forget everything between sessions. Every new conversation, you re-explain your project from scratch — stack, schema, features, decisions, conventions. Mid-conversation, context decays around prompt 12-15, causing wrong suggestions that waste time and tokens to fix.

**Vibe Brain eliminates both problems.** The agent reads a persistent brain file, stays oriented throughout the session, and never asks you to re-explain.

---

## Per-Session Token Cost Comparison

### Without Vibe Brain

| Activity | Token Cost | Why It Happens |
|----------|-----------|----------------|
| Re-explaining your project | 3,000 - 8,000 | Agent has no memory. You describe stack, schema, features, context every time. |
| Agent asks follow-up questions | 1,000 - 2,000 | Even after explanation, agent needs clarifications it should already know. |
| Wrong suggestions from missing context | 3,000 - 5,000 | Agent doesn't know your patterns, makes suggestions that violate your conventions. |
| Re-explaining past decisions | 1,000 - 2,000 | "Why did we use X instead of Y?" conversations that already happened last session. |
| Repeating failed approaches | 2,000 - 5,000 | Agent suggests something you already tried and abandoned. No anti-pattern memory. |
| **Total per session** | **10,000 - 22,000** | |

### With Vibe Brain

| Activity | Token Cost | Why It Costs This |
|----------|-----------|-------------------|
| Selective brain load (core sections) | 1,500 - 3,000 | Only loads Identity, Preferences, Active Context, Threads, Patterns. Not the full brain. |
| Session logging (~12 entries × ~40 tokens) | ~500 | Lightweight timestamped entries. Never exceeds 3 lines per action. |
| Mid-session refresh (1-2× per session) | 1,000 - 2,000 | Re-reads ~1,000 tokens of key context. Prevents decay at prompt 12+. |
| Compression (amortized — triggers every 2-3 sessions) | ~800 | Merges session buffer into brain. Only fires when buffer hits 50 lines. |
| Health check (amortized — triggers every 10-15 sessions) | ~300 | Verifies brain matches reality. Extremely infrequent. |
| **Total per session** | **4,100 - 6,600** | |

### Savings

| Metric | Without | With Vibe Brain | Saved |
|--------|---------|----------------|-------|
| **Tokens per session** | 10,000 - 22,000 | 4,100 - 6,600 | **55-70%** |
| **Tokens over 10 sessions** | 100K - 220K | 41K - 66K | **~60%** |
| **Tokens over 50 sessions** | 500K - 1.1M | 205K - 330K | **~60%** |
| **Tokens over 200 sessions** | 2M - 4.4M | 820K - 1.32M | **~60%** |

### Dollar Savings (at Claude Opus 4.6 pricing)

Based on $5 per million input tokens (Claude Opus 4.6 standard pricing, Feb 2026):

| Sessions | Without | With Vibe Brain | You Save |
|----------|---------|----------------|----------|
| 10 | $0.50 - $1.10 | $0.21 - $0.33 | ~$0.30 - $0.77 |
| 50 | $2.50 - $5.50 | $1.03 - $1.65 | ~$1.50 - $3.85 |
| 200 | $10.00 - $22.00 | $4.10 - $6.60 | ~$5.90 - $15.40 |
| 500 | $25.00 - $55.00 | $10.25 - $16.50 | ~$14.75 - $38.50 |

> **Note:** Dollar savings scale with usage but remain modest because LLM pricing is efficient per-token. The real ROI is in **developer time saved** ($1,250+ over 50 sessions at $50/hr) and **mistakes prevented** — see below.

---

## Protocol Frequency Analysis

How often does each protocol actually fire, and is it worth it?

### Selective Loading (Protocol 2)

| | Detail |
|---|--------|
| **Fires** | Every session start |
| **Cost** | ~1,500 - 3,000 tokens |
| **What it prevents** | 5,000 - 10,000 tokens of manual re-explanation |
| **ROI** | **3-5x return** |
| **Verdict** | ✅ Essential. Pays for itself every single session. |

Without selective loading, you'd load the entire 500-line brain (~8,000 tokens) or re-explain manually (10,000+ tokens). Selective loading reads only ~150 lines of core sections.

### Mid-Session Refresh (Protocol 3)

| | Detail |
|---|--------|
| **Fires** | Every 10 user prompts |
| **Cost** | ~1,000 tokens per refresh |
| **Frequency** | 1-3 times per session (avg session: 15-30 prompts) |
| **What it prevents** | Context decay → wrong suggestions (3,000 - 5,000 tokens each to fix) |
| **ROI** | **3-5x return per refresh** |
| **Verdict** | ✅ Essential. One prevented mistake pays for 3-5 refreshes. |

Context decay is the #1 cause of wasted tokens in long conversations. Without refresh, the agent at prompt 20 has functionally forgotten what you discussed at prompt 3. The refresh costs 1,000 tokens but prevents one 5,000-token mistake.

**Could the interval be higher?** Yes — 15 prompts would still work for most sessions. But 10 provides a safety margin for complex sessions where context shifts rapidly. The cost difference (1-2 refreshes vs 2-3) is only ~1,000 tokens.

### Progressive Compression (Protocol 8)

| | Detail |
|---|--------|
| **Fires** | When SESSION.md hits 50 lines |
| **Cost** | ~2,000 tokens (read session + write to brain) |
| **Frequency** | Every 2-3 sessions (most sessions produce 15-30 log lines) |
| **What it prevents** | Unbounded session growth → context overflow → data loss |
| **ROI** | **Not a savings protocol — it's a data integrity protocol** |
| **Verdict** | ✅ Essential. Without it, session data is lost between conversations. |

Compression isn't about saving tokens — it's about not losing information. Without compression, your session buffer would grow to 200+ lines across sessions and either overflow the context window or get silently dropped.

**Could the threshold be higher?** 75 lines would reduce compression frequency by ~30% but increases crash risk (more data sitting in the uncompressed buffer). 50 lines is the optimal balance.

### Brain Health Check (Protocol 5)

| | Detail |
|---|--------|
| **Fires** | Every 5th compression |
| **Cost** | ~3,000 - 5,000 tokens (scans schema, API, features, dependencies) |
| **Frequency** | Every 10-15 sessions |
| **Amortized cost** | ~300-400 tokens per session |
| **What it prevents** | Brain drift → stale info → cascading wrong suggestions (10,000+ tokens) |
| **ROI** | **25-30x return** |
| **Verdict** | ✅ Essential but infrequent. Extremely high ROI when it catches drift. |

A health check that catches one stale database table or missing API endpoint prevents hours of debugging from wrong suggestions. The 4,000-token cost every 10-15 sessions is negligible compared to the 10,000+ token cost of one drift-caused mistake.

### Session Logging

| | Detail |
|---|--------|
| **Fires** | After every meaningful action |
| **Cost** | ~40 tokens per entry, ~500 tokens per session |
| **What it prevents** | Lost work context between sessions |
| **ROI** | **Infinite — this IS the brain's input** |
| **Verdict** | ✅ Essential. This is the raw data that makes everything else work. |

---

## Time Savings (The Real Value)

Token savings are real but modest in dollar terms. The actual value is **developer time**:

| Without Vibe Brain | With Vibe Brain | Time Saved |
|-------------------|-----------------|------------|
| 2-5 min re-explaining project each session | 0 min — agent already knows | **2-5 min/session** |
| 5-15 min fixing wrong suggestions from missing context | Rare — agent has full context | **5-15 min/session** |
| 3-10 min re-discussing past decisions | 0 min — decisions are logged | **3-10 min/session** |
| 5-20 min retrying failed approaches | 0 min — anti-patterns prevent this | **5-20 min/session** |
| 2-5 min finding where you left off | 0 min — threads + save state | **2-5 min/session** |
| **Total per session** | | **17-55 min saved** |

Over 50 sessions at an average of 30 minutes saved per session: **25 hours of developer time recovered.**

At $50/hr, that's **$1,250 in productivity** from a free tool.

---

## Quality Improvements (What Metrics Can't Capture)

| Without Vibe Brain | With Vibe Brain |
|-------------------|-----------------|
| Agent suggests patterns you explicitly rejected | Anti-Patterns section prevents re-suggestion |
| Agent forgets your naming conventions | Patterns & Conventions always loaded |
| Agent doesn't know your deployment process | Environments section keeps it informed |
| Agent suggests libraries you already evaluated and rejected | [DEEP] decision tags record alternatives |
| Agent loses track of multi-session work | Thread tracking carries context forward |
| Agent doesn't know which files change most | Hot Paths give proactive context loading |
| Session crashes lose unsaved context | Atomic compression prevents data loss |
| Secrets accidentally written to context | Security protocol: variable names only |

---

## Overhead Breakdown

Where do the Vibe Brain tokens actually go?

```
Per-session overhead: ~4,100 - 6,600 tokens

├── Selective Load ............ 1,500 - 3,000  (36-45%)
├── Session Logging ........... ~500           (8-12%)
├── Mid-Session Refresh ....... 1,000 - 2,000  (24-30%)
├── Compression (amortized) ... ~800           (12-19%)
└── Health Check (amortized) .. ~300           (5-7%)
```

**73-75% of overhead is reading.** Writing (logging, compression) is only 25-27%. This means the system is read-heavy by design — it prioritizes keeping the agent informed over recording data.

---

## When Vibe Brain Saves The MOST

| Scenario | Savings Multiplier |
|----------|-------------------|
| Long-running projects (50+ sessions) | 🟢 Maximum — brain compounds knowledge over time |
| Complex projects (20+ DB tables, 10+ integrations) | 🟢 Maximum — too much to re-explain manually |
| Team handoffs (multiple developers using same repo) | 🟢 Maximum — shared brain eliminates onboarding |
| Projects with many past decisions | 🟢 Maximum — decision history prevents revisiting |
| Short/simple projects (5 sessions, 1 framework) | 🟡 Moderate — brain setup cost amortizes over fewer sessions |
| One-off scripts or throwaway code | 🔴 Minimal — project isn't complex enough to forget |

---

## TL;DR

- **60% fewer tokens per session** on average
- **25+ hours saved** over 50 sessions in developer time
- **Every protocol pays for itself 3-30x** in prevented mistakes
- **Health checks fire every 10-15 sessions** — negligible cost, massive drift prevention
- **The real value isn't tokens — it's zero wrong suggestions, zero re-explanations, zero lost context**
