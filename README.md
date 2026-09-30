# banned.bz – case study

A personal project from 2025 (April to May, about 70 commits): a system that tracked Telegram channels
spreading illegal content and pushed them through Telegram's reporting process until they were taken
down.

**This repository documents the project and contains no code.** The reasons are explained
[below](#why-the-code-is-not-published).

## The problem

Channels that distribute illegal content, such as pirated material, scams and worse, are easy to find on
Telegram and often stay online for a long time. Reporting one by hand is slow, and a single report
rarely leads to action. When a channel is closed, a copy usually reappears under a new name within
hours.

## What the system did

The project had three parts:

- **Control bot** (python-telegram-bot): the operator sent channel links to a private bot and used
  commands to see the tracked channels, their status and the report history.
- **Monitoring** (Hydrogram userbot): scheduled jobs checked whether each tracked channel, bot or
  message was still reachable, and marked it as closed once Telegram removed it. The system also told
  channels apart from users, bots and single messages, so private accounts were never targeted.
- **Report drafting** (local LLM through Ollama): for each channel the system wrote a report that
  described the violation, instead of sending a generic template.

Every tracked link and report was stored in SQLite, so the system could tell when a channel had been
closed and how long it took.

```
operator ──► control bot ──► SQLite (links, reports) ◄── status checks (userbot)
                                     │
                                     └──► report drafting (LLM) ──► Telegram reporting process
```

## What I learned

- **Status tracking mattered most.** Knowing which channels were already closed avoided wasted reports
  and showed how long each takedown took.
- **Takedowns don't last.** Closed channels came back under new names, so the problem is structural and
  cannot be solved by reporting alone.
- **Automation and abuse look the same.** The platform cannot tell automated reports against an illegal
  channel from the same reports against a legitimate one. This is what made the project work, and it is
  also why it should not be public.

## Why the code is not published

The code does not know whether a channel is illegal: it acts on whatever link it is given. Published, it
would be just as effective at taking down a journalist's channel, a competitor or a person being
harassed. Automated mass reporting also breaks Telegram's Terms of Service. The source therefore stays
private.

To report illegal content, use the official channels:

- Telegram: [@notoscam](https://t.me/notoscam) for scams, and `abuse@telegram.org`
- Italy: [Polizia Postale](https://www.commissariatodips.it) for illegal content

## Tech stack

Python · python-telegram-bot · Hydrogram · SQLite · Ollama · Selenium
