---
name: pulse-todo
description: "Unified task management and scheduling for AI agents. Use when: (1) a commitment is made (I'll do X, 帮你跟进, remember to), (2) checking what's pending (待办, what's left, 还有什么没做), (3) completing or dropping a task, (4) heartbeat/wake-up triggers a scheduling check, (5) a recurring or timed task needs to be created, (6) prioritizing what to do next. Triggers on: TODO, 待办, 记一下, 帮我跟进, remember, don't forget, 承诺, follow up, track this, what should I do, schedule, remind, 提醒, heartbeat check, 打工, next task. NOT for: one-off questions, things being done right now in this turn."
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

## 🔴 有人在等我 (Inbound)
- [ ] 回复 Acontext #510 review
- [ ] 查 GitHub 通知 | repeat: every wake
- [ ] 查虾信 | repeat: every wake

## 🟡 有 deadline (Time-bound)
- [ ] 提醒 Luna 公众号赌约 | due: 2026-03-31 09:45

## 🔵 承诺了要做 (Committed)
- [ ] memex #29 semantic search

## ⚪ 有空就做 (Fill)
- [ ] 打工（scan → pick → implement）
- [ ] 整理知识库

## 🔄 定时任务 (Recurring)
- [ ] daily review | repeat: 每天 3:00 | cron: daily-review
- [ ] morning briefing | repeat: 每天 7:00 | cron: morning-briefing
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

Do not hard-code signal sources in HEARTBEAT.md. "Check GitHub notifications" and "check 虾信" are TODO items with `repeat: every wake`, not heartbeat instructions.

## During Conversations

When talking with the user or working on tasks, maintain awareness:

- User says "do X later" or "帮我跟进" → add to TODO.md immediately
- Discover a new task while working → add to TODO.md
- Finish something → mark [x] or delete
- Situation changes → update priority/section/deadline
- New recurring task with a time → add to TODO.md with `repeat:` AND sync cron

## Setup

When installing this skill for the first time:

1. Generate TODO.md from template if it doesn't exist
2. Simplify HEARTBEAT.md to: "Read TODO.md, execute by priority"
3. Migrate existing tasks from old HEARTBEAT.md into TODO.md
4. Migrate cron-only tasks into TODO.md with `cron:` attribute
5. Update the old todo skill reference to point here

## What This Replaces

- Scattered checklists in HEARTBEAT.md
- Separate cron jobs with no visibility in task list
- The old todo skill (basic tracking without scheduling)
- Manual "go do work" prompts from the user
