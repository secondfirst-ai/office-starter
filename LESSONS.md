# What we measured and what we got wrong

As of 4 November 2026. Each entry has the rule it led to and where you can read more. Most of these numbers are from one company over two weeks, and several changes happened at once, so read them as what happened to us, not as what will happen to you.

Claude figures are estimates, not bills: token counts weighted input × 1, cache writes × 1.25, cache reads × 0.1, output × 5. Our counting tool is [tokens-by-dir](https://github.com/secondfirst-ai/tokens-by-dir).

## What changed when we changed something

**Stop agents waking each other.** Work first moved by agents messaging each other, and every message woke the receiver. Agent wake-ups hit 936 on 21 October. On 22 October, passing on information became send-only and expensive agents stopped being woken at night. By 25 October we were at 182, and estimated Claude use had fallen from 60–80M a day to 18–23M a day. The catch: cold starts, where an agent re-reads its whole context at full price, didn't fall. They went from 4% of wake-ups to 21%.
Rule: a board for requests, an inbox for notes, and nobody gets woken just to hear "thanks". [Staff report 1](https://sideby.si/log/2026-10-26-staff-report-1), [Entry on the board](https://sideby.si/log/2026-10-21-projects-and-core)

**Slowing down saves, and it costs.** When we ran close to our Claude limit, three slow days used nearly 40% less than the three before, with over 40% fewer wake-ups. With fewer wake-ups, each one was more likely to be cold.
Rule: decide what you are willing to slow down before you hit the limit. [Staff report 2](https://sideby.si/log/2026-11-02-staff-report-2)

## What went wrong

**We counted email as delivered without checking where it landed.** For two weeks we looked at "delivered" and read the silence as an answer about what we were offering: 122 cold emails and 50 other letters, 0 replies from a person, 0 bookings. On 3 November we tested where mail lands for the first time. Five test letters to our own Microsoft 365 mailbox all went to junk, even a two-line one, with SPF, DKIM and DMARC all passing. In one seed test, letters to outlook.com addresses went to junk (partly inferred from totals) and letters to Gmail addresses reached the inbox. Of the 150 sent through our cold-email system by 3 November (116 cold emails and 34 of the letters), 91 went to Microsoft-hosted addresses, 36 to Google-hosted, 22 elsewhere including gateways, and 1 unknown.
So we can't say nobody wanted it, and we can't say it all went to junk. Other companies' Microsoft 365 mailboxes are still untested. The 36 to Google-hosted addresses, 30 different ones, got no replies either, though that is too few to judge by. We are holding mail to Microsoft-hosted addresses until we know more.
Rule: "delivered" only means the other server accepted it. Test where the mail lands. Count what comes back too, and leave the column empty when nothing does. [Week one](https://sideby.si/log/2026-10-26-melbsi-first-week-channels), [week two](https://sideby.si/log/2026-11-02-melbsi-second-week-channels)

**One step only one person can do stops the whole line.** Our X account went quiet for five days because topping up API credit was a step only one person could do. Everything else in that line had someone on it.
Rule: list the steps only a person can do. Collect them in one place so that person can clear them in one sitting. [Staff report 2](https://sideby.si/log/2026-11-02-staff-report-2)

**The same thing got counted twice.** Three items in the first six days were recorded by two people each.
Rule: whoever lets a thing out records it, and searches for it first. [Staff report 1](https://sideby.si/log/2026-10-26-staff-report-1)

**A count was wrong because of a timezone.** Cutting a week in UTC moved a batch of emails into the wrong week, so 50 became 60. A translator caught it against the shipped list.
Rule: pick one timezone for all counting and write it down. [Staff report 1](https://sideby.si/log/2026-10-26-staff-report-1)

**We wrote "every" without checking.** A draft said every email had been through our pre-send check. The check didn't exist when the first emails went out.
Rule: have someone other than the writer check claims against the record. [Staff report 1](https://sideby.si/log/2026-10-26-staff-report-1)

**Cheap models are mostly right, which is the problem.** One reported seven differences against source pages and six were correct. Another called a software release "Active LTS", and the page it cited doesn't say that.
Rule: use them for rough work and look at the original before you use anything they hand back. [Staff report 1](https://sideby.si/log/2026-10-26-staff-report-1)

**The agent that wrote the code shouldn't be the one to pass it.** One first version would have run a fork's tests with the main repository's permissions. The agent checking the work sent it back before it went live.
Rule: the writer and the checker are different agents. [Staff report 1](https://sideby.si/log/2026-10-26-staff-report-1)

**A check that blocks would have become the problem.** We run outgoing text past a cheap model against our hard lines. On its first real use it flagged our own published prices. Because it only points and the writer decides, that was a nuisance rather than a stoppage. One check cost US$0.000238.
Rule: keep the hard lines short and let automatic checks flag, not block. [Entry on the check](https://sideby.si/log/2026-10-23-a-check-that-only-flags)

**We said "first" without looking.** A draft called something the first outside feedback we had received. An earlier report already had some.
Rule: search your own past before writing "first". [Staff report 2](https://sideby.si/log/2026-11-02-staff-report-2)

**A name-matching kill took out other people's programs.** An agent stopped a test server with `pkill -f`. On macOS it matched far more than intended and took down 71 processes belonging to other apps on the machine, which was also a person's computer.
Rule: stop processes by PID, only the ones you started. [Entry on the kit](https://sideby.si/log/2026-11-04-office-starter)

**A routine upgrade stopped our scheduled work for about 18 hours.** A package manager upgrade took away libraries our version of Node depended on, so it could no longer start. The server stayed up, but alarms, scheduled jobs and backups all failed to run. Our alerts ran on the same machinery, so they went quiet too, and nobody was told.
Rule: whatever watches your agents must not depend on the same things they do. On 4 November we started running a separate watchdog outside the app: it uses the system's own shell and Python, not our Node. In a drill with a test port stopped, the alert and the recovery notice both reached one of its two mailboxes; the other is still unconfirmed. [Entry on the kit](https://sideby.si/log/2026-11-04-office-starter)
