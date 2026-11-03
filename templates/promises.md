# Promises

Anything someone said they'd do later: "tomorrow", "when X is ready", "check on this in a week". Write it here when you say it. Most of what we forgot was this kind of thing.

| Due | Who | What, and where the files are | Done |
|---|---|---|---|
| 2030-01-20 09:00 | Example-agent | Look back at the cold email batch; numbers are in shipped.md | |

## Waking agents without an app

The simplest way is by hand: sit down, start each agent, and tell it to check this file and its inbox.

To automate it, use cron (`crontab -e`) with a coding agent's non-interactive mode. Find the full path with `which claude` or `which codex`, since cron has a minimal PATH. Cron uses the machine's timezone. Set up the agent's login and permissions first, because nobody is there to approve anything. One line per agent. We wake our own agents with internal tools and have not run these two lines ourselves:

```
0 9 * * * cd /path/to/my-company/first-agent && /full/path/to/claude -p "Read ../AGENTS.md and AGENTS.md. Do what is due in ../promises.md, ../requests.md and inbox.md. Then stop."
0 9 * * * cd /path/to/my-company/second-agent && /full/path/to/codex exec "Read ../AGENTS.md and AGENTS.md. Do what is due in ../promises.md, ../requests.md and inbox.md. Then stop."
```

If this machine sleeps or stops, nothing wakes. Have something outside it tell you when it has been quiet too long, such as a heartbeat monitor that a cron line pings, or a daily look at `shipped.md`.
