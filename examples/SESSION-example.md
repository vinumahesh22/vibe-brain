# Session Buffer
> Session: 2026-03-15 13:00 PST
> Project: TaskFlow
> Lines: 24 / 50 max
> Context refreshes: 1

## Active Threads

### [THREAD-1] Real-time migration: Vercel KV → Ably
- **Started**: 2026-03-15 13:00
- **Status**: in-progress
- **Context**: Vercel KV pub/sub ~2s latency, unacceptable for Kanban drag-and-drop. Migrating to Ably for sub-100ms.
- **Files**: lib/realtime.ts, hooks/useRealtimeTasks.ts, types/realtime.ts
- **Last action**: Benchmarked KV latency, extracted realtime types, added perf logging
- **Next step**: Install @ably/realtime, create prototype channel, benchmark

### [THREAD-2] Notifications system design
- **Started**: 2026-03-15 14:30
- **Status**: parked
- **Context**: User wants in-app + email digest notifications. Parked until real-time migration completes.
- **Next step**: Design notification schema + tRPC router after Ably is live

## Save State
> Stopped mid-task: Benchmarking Vercel KV latency for real-time migration
> Next action: Install @ably/realtime, create prototype channel, benchmark vs KV
> Open files: lib/realtime.ts, hooks/useRealtimeTasks.ts
> Blocked on: nothing

---

## Session Log

### 14:30 CONTEXT
User priority: fix real-time latency first, then notifications. Billing and AI
stable — no active work needed.

### 14:15 FEAT
Added performance logging to realtime hooks. Measuring pub/sub round-trip
latency per event. File: `hooks/useRealtimeTasks.ts`

### 14:00 DISCOVERY
Vercel KV pub/sub confirmed ~2s latency on task status changes. Users see stale
Kanban state during drag-and-drop. Root cause: KV not designed for real-time.

### 13:45 DECISION
Chose Ably over Pusher for real-time replacement. Reasons: sub-100ms latency,
built-in presence (shows who's viewing a task), generous free tier (6M messages/mo).

### 13:30 FIX
Fixed task reorder persistence — positions not saving when dragging between
columns. Missing `workspaceId` in Prisma where clause. File: `server/routers/task.ts`

### 13:15 REFACTOR
Extracted real-time types into `types/realtime.ts`. Cleaned up hook imports.
Preparing for clean swap from Vercel KV → Ably.

### 13:00 TEST
Full test suite after reorder fix: 142/142 passing. `npm test` — all green.
Coverage: 78%.
