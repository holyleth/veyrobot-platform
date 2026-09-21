<div align="center">

<img src="https://veyrobot.xyz/images/logo.png" width="110" alt="VeyroBot">

# VeyroBot

**Stop stacking bots. Run your Discord server from one control center.**

VeyroBot is a Discord bot that runs a server's moderation, security and community features from a single web dashboard and scores the server's health.

Moderation, anti-nuke, automod, tickets, leveling, music, stream and content alerts (YouTube, Twitch, Kick, Reddit, Bluesky, RSS and podcasts),
scheduled messages and analytics — one web panel, one permission model, one database.

[![Website](https://img.shields.io/badge/veyrobot.xyz-3B82F6?style=flat-square)](https://veyrobot.xyz)
[![Add to Discord](https://img.shields.io/badge/Add%20to%20server-5865F2?style=flat-square&logo=discord&logoColor=white)](https://veyrobot.xyz/invite)
[![Support](https://img.shields.io/badge/Support-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/NKYe4WbyaN)
![Languages](https://img.shields.io/badge/7%20languages-1a1a1a?style=flat-square)

<sub>This repository is a public overview of the product. <b>The source code is not published here.</b></sub>

</div>

---

## The problem

Most servers end up running five different bots: one for moderation, one for tickets, one for
leveling, one for stream alerts, one for music. Five dashboards, five permission systems, five
things that break independently, and no single place that knows what your server actually looks like.

VeyroBot is the other approach: **one bot, one panel, one database** — and a panel that reads the
bot's live state instead of a copy of it.

---

## What it looks like

### Server Health — a real score, not a vanity number

Scans your Discord permissions, protection modules and misconfigurations, and tells you exactly what
is wrong and where to fix it. One-click fixes with a 24-hour undo window.

<img src="docs/images/overview-server-health.webp" alt="Server Health scoring and module grid">

### Anti-Raid Shield

Join-rate thresholds trigger an automatic lockdown; `/unlock` restores every channel's original
permissions from a snapshot taken before the lock.

<img src="docs/images/anti-raid.webp" alt="Anti-raid configuration">

### Analytics that answer questions

Message volume, command usage, member retention, join/leave breakdown and activity heatmaps —
rendered in your server's own timezone. Scheduled daily, weekly or monthly reports to a channel.

<img src="docs/images/analytics.webp" alt="Server analytics charts">

### Welcome Studio with a live preview

Design the welcome card and see the actual Discord message render as you type.

<img src="docs/images/welcome-studio.webp" alt="Welcome card studio with live Discord preview">

---

## Features

| Area | What's included |
|---|---|
| **Security** | Anti-nuke with audit-log thresholds and one-click role restoration · anti-raid lockdown · recommended security setup with undo · server backup |
| **Moderation** | Warning tiers with automatic penalties · timeout, kick, ban · full audit logging · web moderation from the panel |
| **AutoMod** | Discord-native AutoMod rules configured from the panel, plus VeyroBot's own filters |
| **Members** | Welcome and goodbye cards · autoroles · timed reaction roles · leveling and voice XP · birthdays · achievements · invite tracking |
| **Notifications** | YouTube, Twitch, Kick, Reddit, Bluesky, RSS and podcasts — one card template, live stream cards that update and close out |
| **Community** | Tickets with typed categories · application forms · suggestions · giveaways · starboard · sticky messages · custom commands |
| **Scheduling** | Timed messages · reminders · events · automated analytics reports |
| **More** | Music with a DJ studio · Google Gemini AI · stats channels · emoji gallery · setup wizard |

Every module can be turned on or off individually, and access is decided in one place rather than
scattered across the codebase.

---

## How it's built

VeyroBot runs as **a single Node.js process**. The Discord bot and the Express web panel are not
separate services — they share the same live `discord.js` client and the same SQLite handle.

That means the panel never shows stale state, because there is no copy to fall out of sync. It also
means a mistake in a web route can take the bot down with it, which is a tradeoff made on purpose
rather than by accident.

```mermaid
%%{init: {'theme':'neutral', 'flowchart':{'curve':'basis'}}}%%
flowchart LR
    G["Discord Gateway"] --> C
    B["Browser"] --> W

    subgraph P ["one Node.js process"]
        direction TB
        C["discord.js client"]
        W["Express dashboard"]
        E["access engine<br/>one decision point"]
    end

    C --> E
    W --> E
    E --> D[("SQLite<br/>encrypted volume")]
```

A few things that are deliberate:

- **The dashboard runs under an enforced Content-Security-Policy** with no `unsafe-inline`. Every
  event handler lives in an external file. Stricter than required, on purpose.
- **Data at rest sits on an encrypted volume**, and the service refuses to start if that volume
  isn't mounted — a bot that won't boot beats a bot that boots on an empty database.
- **Payment webhooks** are verified by signature, checked for idempotency, and matched against the
  store before anything is granted.
- **Seven interface languages** — Turkish, English, Spanish, German, Portuguese, Russian, Hindi —
  with no partially translated pages.

---

## Frequently asked questions

### What is VeyroBot?

VeyroBot is a Discord bot that runs a server's moderation, security and community features from a single web dashboard and scores the server's health. The dashboard is available in seven languages at [veyrobot.xyz](https://veyrobot.xyz/en).

### What does it replace?

Most servers run separate bots for moderation, tickets, leveling, stream alerts and music. VeyroBot covers those from one bot with one web panel, one permission model and one database.

### How does the server health score work?

It scores a server from 0 to 100 from its real Discord permissions and settings — dangerous `@everyone` permissions, a low verification level, other bots holding Administrator, settings that silently do nothing — and gives the reason for every lost point. One-click fixes are applied only after you confirm and can be undone for 24 hours.

### Is VeyroBot free?

Adding VeyroBot is free. VeyroBot is still growing, so which features are free and which are Premium may change over time; the [Premium page](https://veyrobot.xyz/en/premium) always shows the current list.

### How does VeyroBot compare with MEE6, Dyno or Carl-bot?

Each comparison page quotes the competitor's own page and shows the date of the quote: [MEE6](https://veyrobot.xyz/en/compare/mee6), [Dyno](https://veyrobot.xyz/en/compare/dyno), [Carl-bot](https://veyrobot.xyz/en/compare/carl-bot) and [all three side by side](https://veyrobot.xyz/en/compare/mee6-dyno-carl-bot).

### Which languages does it support?

Turkish, English, Spanish, German, Portuguese, Russian and Hindi. Every dashboard page exists in all seven — there are no partially translated pages.

### Is the source code available?

No. This repository is a public product overview and VeyroBot is proprietary software; see [NOTICE](NOTICE.md).

---

## Links

| | |
|---|---|
| Website | [veyrobot.xyz](https://veyrobot.xyz) |
| Documentation | [veyrobot.xyz/en/docs](https://veyrobot.xyz/en/docs) |
| Features | [veyrobot.xyz/en/features](https://veyrobot.xyz/en/features) |
| Command list | [veyrobot.xyz/en/commands](https://veyrobot.xyz/en/commands) |
| Blog | [veyrobot.xyz/en/blog](https://veyrobot.xyz/en/blog) |
| Release notes | [veyrobot.xyz/en/changelog](https://veyrobot.xyz/en/changelog) |
| Premium | [veyrobot.xyz/en/premium](https://veyrobot.xyz/en/premium) |
| Status | [veyrobot.xyz/en/status](https://veyrobot.xyz/en/status) |
| Support server | [discord.gg/NKYe4WbyaN](https://discord.gg/NKYe4WbyaN) |

---

## About this repository

This repository exists to document and present the product. **It contains no source code**, and none
is planned. VeyroBot is proprietary software — see [NOTICE](NOTICE.md).

Found a bug or have a feature request? The [support server](https://discord.gg/NKYe4WbyaN) is the
fastest route; issues here are welcome too.

<div align="center">
<sub>© VeyroBot · All rights reserved</sub>
</div>
