+++
title = "One Year of Homelab — The Services I Actually Use"
date = 2026-03-07
draft = false
description = "After a year of running a homelab and a stretch of time where I barely touched it, here are the self-hosted services that genuinely made a difference in daily life — from media streaming and photo management to backups and push notifications."

[taxonomies]
categories = ["Technology", "Self-hosting"]
tags = ["homelab", "self-hosting", "jellyfin", "immich", "vaultwarden", "ntfy", "uptime-kuma", "restic", "backup", "media-server"]

[extra]
#cover.image = "images/homelab-one-year-#cover.png"
#cover.alt = "One Year of Homelab — The Services I Actually Use"
+++

# One Year of Homelab — The Services I Actually Use

I just came out of a pretty unusual stretch of time. My wife and I were expecting, and for months I barely touched my homelab. No new experiments, no new services — just life happening at full speed.

But then the baby arrived 🎉

Looking back, I'm genuinely glad I had set everything up beforehand. Not every service proved its worth during this period — but some of them quietly became part of our everyday family life. What follows is an honest list, not a "cool things I run" showcase.

---

## 1. Media Streaming — The One Everyone Actually Uses

Watching movies and shows is still the simplest, most universal form of entertainment. What's interesting is that while most services in my homelab are used only by me, the media server gets used by the whole family — my wife, parents, in-laws.

During the short breaks you get while taking care of a newborn, being able to hit play on something familiar — no login screens, no ads, no buffering — is a genuine lifesaver. I'm running **Jellyfin**: open-source, lightweight, and far more stable than I expected.

---

## 2. Photo Management — The Most Valuable Data Right Now

I still use Google Photos, but having a local copy of all our family photos at home gives me a peace of mind that's hard to put into words. At this stage of life, family photos — especially the early moments with a newborn — are the most irreplaceable digital assets I have.

**Immich** is my main solution, paired with a few custom scripts to automate syncing and organization. It's not perfect, but it does the job — and the data lives on my own hardware.

---

## 3. Mail Archiving, Summarization & Notifications — The Thing I Built Myself

This one is a bit different from the rest, because I built it rather than installing an existing tool.

The system automatically pulls mail from my providers (Gmail and a few others), stores it locally, then uses an LLM to summarize, categorize, and suggest actions. Every two hours, the machine runs a sync and I get a clean digest — instead of wading through dozens of emails manually.

The stack is simple: **mbsync** to fetch mail, **notmuch** as the database, and an LLM service to handle the language side.

I know there are plenty of tools out there that do something similar. But for someone who can code, writing a small app that does exactly what you want is often faster than installing a tool and fighting with config until it behaves the way you need. I'm not recommending this approach to everyone — it's just the one I chose.

---

## 4. Vaultwarden — Self-Hosted Password Management

We live in an era of a thousand accounts, a thousand passwords, secret phrases, passkeys, security questions, private notes. You *need* a password manager. There's really no way around it.

But it's even better when that sensitive data lives on your own server, rather than relying entirely on a third party. **Vaultwarden** is a self-hosted, Bitwarden-compatible server — polished clients across all platforms, and it just works.

---

## 5. Uptime Kuma — So You Never Ask "Is That Still Running?"

Once you have a handful of services running around the clock, the worst possible situation is discovering one has been down for hours only when you actually need it.

**Uptime Kuma** is the most beginner-friendly monitoring solution I've used. It takes minutes to set up, has a clean dashboard, and supports notifications across many channels. I don't think about it much day-to-day — which is exactly the point. It's a silent guardian.

---

## 6. Backups — Because Nobody Wants to Learn This Lesson the Hard Way

Once your setup grows to a reasonable scale, the question is no longer "will I ever lose data?" but "when something goes wrong, can I recover — and how fast?"

I'm using **restic** with **Cloudflare R2** (free tier). Since I want to stay within the free limits, I wrote custom scripts to back up only what truly matters, nothing more. Equally important: there's a recovery script too — because a backup you've never tested isn't really a backup.

---

## 7. Ntfy — The Phone Is the Bridge Between Server and Human

I can't sit next to my server. I can't open a dashboard every 30 minutes to check on things. Life doesn't allow that — especially not these days.

So the phone is the real communication channel between my server and me. When something goes wrong — a service dies, a backup completes, a cron job fails — I want to know immediately, wherever I am, without having to go looking.

**Ntfy** solves exactly that. It's a simple HTTP endpoint: any script or service in my stack can send a push notification to my phone with a single `curl` command. No complex integrations, no SDK, no third-party account required.

My reason for choosing Ntfy is entirely pragmatic: I installed it first, it worked well, and I've never had a reason to try anything else. Sometimes that's the best endorsement a tool can get.

---

## Wrapping Up

Looking back, the services that proved genuinely useful weren't the ones that sounded most impressive at setup time. They were the ones stable enough to run without constant attention, and useful enough that other people in the family could benefit from them too.

There's still plenty I want to explore in my homelab. But for now, there's a baby who needs looking after — and thankfully, the system I built before is handling the rest 🙂
