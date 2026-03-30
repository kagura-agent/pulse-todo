---
name: pulse-todo
description: "Unified task management and scheduling for AI agents. Use when: (1) a commitment is made (I'll do X, 帮你跟进, remember to), (2) checking what's pending (待办, what's left, 还有什么没做), (3) completing or dropping a task, (4) heartbeat/wake-up triggers a scheduling check, (5) a recurring or timed task needs to be created, (6) prioritizing what to do next. Triggers on: TODO, 待办, 记一下, 帮我跟进, remember, don't forget, 承诺, follow up, track this, what should I do, schedule, remind, 提醒, heartbeat check, next task. NOT for: one-off questions, things being done right now in this turn."
---

# Pulse TODO

One list. All tasks. Every wake-up, check the pulse.

## Core Principle

**Everything is a TODO.** Inbound messages to respond to, deadlines, commitments, recurring checks, idle-time work — they're all tasks in one list. The difference is priority and attributes, not where they live.

## TODO.md Location

`TODO.md` in workspace root. This is the single source of truth for all pending work.

## TODO.md Format

```markdown
# TODO

## 🔴 Waiting on me (Inbound)
- [ ] Reply to PR #510 review
- [ ] Check notifications | repeat: every wake
- [ ] Check inbox | repeat: every wake

## 🟡 Has deadline (Time-bound)
- [ ] Remind user about deadline | due: 2026-03-31 09:45

## 🔵 Committed
- [ ] Implement semantic search for project X

## ⚪ When free (Fill)
- [ ] Contribute to open source projects
- [ ] Organize knowledge base

## 🔄 Recurring
- [ ] Daily review | repeat: daily 3:00 | cron: daily-review
- [ ] Morning briefing | repeat: daily 7:00 | cron: morning-briefing
```

### Attributes

Append after `|`:
- `repeat: <schedule>` — recurring task; don't delete on completion, reset for next cycle
- `due: <YYYY-MM-DD>` or `due: <YYYY-MM-DD HH:MM>` — deadline
- `cron: <job-name>` — linked to an OpenClaw cron job (must stay in sync)

### Rules

Written here so the agent sees them every time TODO.md is read:

1. **Priority = section order.** 🔴 > 🟡 > 🔵 > ⚪ > 🔄. Always work top-down.
2. **Add immediately.** When a commitment is detected — from the user, from a notification, from your own work — add it before doing anything else.
3. **One source of truth.** If it's not in TODO.md, it doesn't exist. No mental notes.
4. **Done = verified done.** PR submitted ≠ done. PR merged = done.
5. **Repeat tasks reset.** After completing a `repeat:` task, uncheck it — don't delete.
6. **Stale = 3 days.** Item untouched for 3+ days? Either do it or delete it.
7. **Move between sections.** Situation changed? Move the item to the right priority section.
8. **Cron sync.** Adding/removing a `cron:` task means updating OpenClaw cron too.

## Scheduling (Heartbeat Wake-Up)

When triggered by heartbeat or any "what should I do next" moment:

1. Read TODO.md
2. Scan 🔴 — anything urgent? Handle it.
3. Scan 🟡 — anything due soon? Handle it.
4. Nothing urgent → pick from 🔵 or ⚪
5. Nothing at all → HEARTBEAT_OK

Do not hard-code signal sources in HEARTBEAT.md. Checking notifications and checking your inbox are TODO items with `repeat: every wake`, not heartbeat instructions.

## During Conversations

When talking with the user or working on tasks, maintain awareness:

- User says "do X later" or "remember to" → add to TODO.md immediately
- Discover a new task while working → add to TODO.md
- Finish something → mark [x] or delete
- Situation changes → update priority/section/deadline
- New recurring task with a time → add to TODO.md with `repeat:` AND sync cron

## Setup

If this is your first time using pulse-todo, follow the setup guide in [setup.md](setup.md).

## What This Replaces

- Scattered checklists in HEARTBEAT.md
- Separate cron jobs with no visibility in task list
- Basic todo tracking without scheduling or priority
- Manual "go do work" prompts from the user
