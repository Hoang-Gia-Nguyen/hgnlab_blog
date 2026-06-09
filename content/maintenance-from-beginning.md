+++
title = "Self-Hosting Isn’t Deployment — Maintenance Is the Real Game"
date = 2026-04-04
description = "Self-hosting is not just about getting services running. Maintenance is the real challenge — and it should be considered from day one."
slug = "self-hosting-maintenance-from-day-one"
draft = false

[extra]
#cover.image = "images/maintenance-from-beginning-#cover.png"
#cover.alt = "Self-Hosting Isn’t Deployment — Maintenance Is the Real Game"

[taxonomies]
tags = ["self-hosting", "devops", "maintenance", "homelab"]
categories = ["Self-hosting"]
+++


---

# When life gets busier than your server

Recently, I started a new job. At the same time, we welcomed a newborn into the family. Naturally, most of my time and attention shifted there.

The little server sitting in the corner — something I used to tinker with almost daily — has been quietly running on its own. The screen hasn’t been turned on in a while.

Until one day, things started asking for attention again.

My expense tracking app went down.  
The Plex server — where my wife watches movies — suddenly became inaccessible.

It’s not that I didn’t know how to fix it. I could roughly guess what was wrong. But to properly investigate and fix it, I’d need a few solid hours of focused time.

And that’s exactly what I didn’t have.

---

# When you realize time is the real constraint

Before, I could spend an entire evening debugging, experimenting, or even rebuilding things from scratch.

Now, the reality is:
- I might get about an hour a day
- Sometimes not even that
- And rarely in one uninterrupted block

That’s when something hit me:

> Some services I had designed with maintenance in mind were easy to fix.  
> Others — the ones I rushed — became a real headache.

Same self-host setup. Completely different maintenance experience.

---

# Maintenance is not what happens after deployment

When people talk about self-hosting, the focus is usually on:
- getting the service up and running
- exposing it to the internet
- making sure it works

But that’s just the beginning.

Maintenance includes:
- Updating systems and applications
- Backups and restoring data
- Monitoring system health
- Debugging issues
- Handling security

And most importantly:

> Maintenance is not something that happens *after* deployment.  
> It has to be considered *from the very beginning*.

---

# What I do now (and what I learned the hard way)

## 1. Updates: don’t upgrade blindly

Updating is fun:
- new features
- bug fixes
- everything feels fresh

But if you can’t roll back, you’re gambling with your system.

For Docker-based services, my approach is:

- Read the changelog before upgrading
- Backup configs and data
- Keep the old container running
- Spin up a new container with the new image
- Only remove the old one after confirming stability

For self-built applications:
- Deploy versions side by side
- Avoid destroying the current version immediately

> A new deployment should not break what’s already working.

---

## 2. Backup & Restore: non-negotiable

If there’s one thing you should never postpone, it’s backups.

Not:
> “I’ll set it up later when I have time”

Because your system might fail **before that moment comes**.

Even a simple setup:
- weekly `rsync`
- copying configs and data to another disk or cloud

…can save you from disaster.

But backups are more than just copying files:

- They consume storage → cost matters
- You need to decide:
  - what must be backed up
  - what you can afford to lose
- You need:
  - retention strategy
  - appropriate frequency

And most importantly:

> Test your restore.  
> Test your restore.  
> Test your restore.

Having backup files doesn’t mean you can actually recover from them.

---

## 3. Observability: don’t let your system fail silently

If you don’t monitor your system, you’ll only find out something is wrong when it’s already broken.

At a minimum, keep track of:
- Disk usage
- RAM / CPU
- Temperature
- Network traffic
- Service uptime

It doesn’t have to be complex. Just enough to:

> Detect issues early → fix them when you still have time.

Instead of:
> Realizing something is broken only when you need it.

---

## 4. Security: you’re not a target — but bots don’t care

A common assumption:

> “My setup is small, no one would bother hacking it.”

Reality:
- No one is targeting you specifically
- But automated bots scan **everything**

All it takes is:
- one exposed port
- one poorly configured service

And suddenly, your home network becomes vulnerable.

Some principles I try to follow:
- Only expose what is absolutely necessary
- Prefer local access or VPN
- Apply the principle of least privilege
- Don’t give services more permissions than they need

Security is a deep field. You don’t need perfection, but:

> Getting the basics right already protects you from a lot of unnecessary trouble.

---

# Maintenance is a design problem

The biggest realization for me:

> Maintenance is not an operations problem.  
> It’s a design problem.

From the beginning, you should be asking:
- How do I update safely?
- How do I roll back?
- What happens if a service crashes?
- What happens if data is lost?

If you don’t answer these early, you’ll be forced to answer them… when things are already broken.

---

# A note to your future self

Three months from now, you:
- won’t remember today’s configs
- won’t remember why things were set up this way
- might accidentally delete something important while cleaning up disk space

And in that moment, you’ll wish:

> “I should have done this more carefully from the start.”

---

# Conclusion

Self-hosting is not a project with an end.  
It’s a system you live with.

And in that system:

- Disks will fill up  
- Services will crash  
- Networks will fail  

It’s not a matter of *if*, but *when*.

So from day one:

> Design for maintenance.  
> Design for failure.

You’re not just deploying a system.  
You’re taking responsibility for running it.
