# banned.bz (case study)

A personal project from 2025, built over April and May across roughly 70 commits. It tracked Telegram
channels that spread illegal content and pushed them through Telegram's reporting process until they got
taken down.

This repository is a write-up of the project. It contains no code, and the reasons for that are at the
[bottom](#why-the-code-isnt-here).

## The problem

Channels that hand out illegal content, pirated stuff, scams and worse, are easy to find on Telegram and
often stay up for a long time. Reporting one by hand is slow, and a single report rarely does anything.
When a channel does get closed, a copy is usually back under a new name within hours.

## What it did

There were three parts to it.

The control bot (python-telegram-bot) was how I used it: I sent channel links to a private bot, and
commands let me see the tracked channels, their status and the report history.

The monitoring side was a Hydrogram userbot. Scheduled jobs checked whether each tracked channel, bot or
message was still reachable, and marked it as closed once Telegram had removed it. It also told channels
apart from users, bots and single messages, so private accounts were never touched.

Report drafting used a local LLM through Ollama. For each channel it wrote a report describing the
violation, rather than sending the same generic template every time.

Every link and report went into SQLite, which is what let it tell when a channel had been closed and how
long that took.

## What I took away from it

The status tracking turned out to be the important part. Knowing which channels were already gone meant
no wasted reports, and it showed how long each takedown actually took.

Takedowns didn't stick. Closed channels came back under new names, so the real problem is structural and
you can't fix it by reporting alone.

And the uncomfortable one: automation and abuse look identical. The platform can't tell automated reports
against an illegal channel from the same reports aimed at a legitimate one. That's exactly what made the
project work, and it's also why I'm not putting the code out.

## Why the code isn't here

The code has no idea whether a channel is illegal. It acts on whatever link you give it. Out in the open
it would be just as good at taking down a journalist's channel, a competitor, or someone being harassed.
Coordinated mass reporting also breaks Telegram's Terms of Service. So the source stays private.

If you need to report illegal content, use the official routes:

* Telegram: [@notoscam](https://t.me/notoscam) for scams, and `abuse@telegram.org`
* Italy: the [Polizia Postale](https://www.commissariatodips.it)

## Built with

Python, python-telegram-bot, Hydrogram, SQLite, Ollama and Selenium.
