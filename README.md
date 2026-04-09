# Pulse TODO

Unified task management and scheduling skill for AI agents. One list, all tasks, every wake-up — check the pulse.

## What Is This?

Pulse TODO is an **AgentSkill** for [OpenClaw](https://github.com/nicepkg/openclaw) / [ClawHub](https://clawhub.com). It gives your agent a single `TODO.md` file as the source of truth for all pending work — commitments, reminders, recurring checks, and idle-time tasks.

For full skill behavior and trigger rules, see [SKILL.md](SKILL.md).

## Install

```bash
# Via ClawHub
clawhub install pulse-todo

# Or manually: copy this directory into your OpenClaw skills folder
cp -r pulse-todo ~/.openclaw/skills/
```

## Features

- **Single source of truth** — one `TODO.md` for all tasks
- **Dependency-based grouping** — "Do it myself" / "Depends on human" / "Waiting on external" + Scheduled
- **Cron integration** — recurring and timed tasks auto-sync with OpenClaw cron jobs
- **Heartbeat-aware** — picks up unscheduled work on agent wake-up
- **Strategic prioritization** — aligns task selection with north-star goals, not just ease
- **Stale detection** — flags items untouched for 3+ days

## Setup

See [setup.md](setup.md) for first-time configuration steps.

## License

MIT

## Star History

<a href="https://www.star-history.com/#kagura-agent/pulse-todo&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=kagura-agent/pulse-todo&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=kagura-agent/pulse-todo&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=kagura-agent/pulse-todo&type=Date" />
 </picture>
</a>