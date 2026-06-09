+++
title = "The Cloudflare Stack: My 'Cheat Code' for Self-Hosting"
date = 2026-04-05
description = "Bypass CGNAT, secure your services, and host for (almost) free. Why Cloudflare is the ultimate toolbox for modern self-hosters."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["cloudflare", "self-hosting", "security"]

[extra]
#cover.image = "images/cloudflare-stack-#cover.png"
#cover.alt = "Cloudflare Stack Illustration"
+++

If you're self-hosting in 2026, especially from a home connection, you've likely hit the "ISP Wall." Whether it's CGNAT blocking your ports or the lack of a static IP, getting your services online safely used to be a headache.

Enter the **Cloudflare Stack**. For me, it's been a total game-changer—a collection of tools that solve almost every infrastructure problem for the cost of a few cups of coffee per year.

### 1. Cloudflare Tunnel: The CGNAT Killer
This is the heart of my setup. Instead of opening ports on my router (which is risky and often impossible with local ISPs), I run a tiny `cloudflared` container. It creates a secure, outbound-only connection to Cloudflare.
- **No Static IP needed:** It doesn't matter if your IP changes every hour.
- **Bypass CGNAT:** Works perfectly even if your ISP hides you behind a private IP.
- **Built-in Security:** You get Cloudflare's DDoS protection and WAF (Web Application Firewall) right out of the box.

### 2. Cheap Domains (.id.vn is a steal!)
A professional setup needs a domain. While `.com` or `.net` can be pricey, "niche" TLDs are incredibly affordable. I picked up mine for around **180,000 VND/year** (less than $8). It’s a small price to pay for having `service.yourdomain.com` instead of a messy IP address.

### 3. Generous Storage: D1 & R2
Cloudflare isn't just for networking anymore.
- **D1 (SQL Database):** Perfect for small apps. I use it for my custom spending tracker. The free tier is so generous I haven't paid a cent yet.
- **R2 (Object Storage):** S3-compatible storage with **zero egress fees**. I use this to offload backups and store media for my serverless apps.

### 4. Serverless Freedom: Workers & Pages
Why run everything on your home server?
- **Cloudflare Pages:** This entire blog is hosted here. It's fast, free, and deploys automatically whenever I push to GitHub.
- **Cloudflare Workers:** I've moved "light" services like my text-sharing bin and expense tracker here. 
Moving these to the edge means my home server doesn't have to stay awake for simple tasks, and maintenance (upgrades/rollbacks) is as easy as a `git push`.

### 5. Zero Trust: The Modern VPN
Traditional VPNs are clunky. With **Cloudflare Zero Trust**, I can put an authentication layer (like Google, GitHub, or an Email OTP) in front of any service. I can access my home dashboard from my phone anywhere in the world without ever "connecting" to a VPN—it just works through the browser, securely. **One important note: this is a security and access tool, not a privacy tool.** While it provides isolation, it's not meant to hide your activity from your ISP or the internet like a traditional "privacy VPN".

### Conclusion
The Cloudflare Stack isn't just about saving money; it's about **removing friction**. It lets me focus on *what* I'm building or hosting, rather than *how* to keep it online and secure. 

If you're just starting your self-hosting journey, this is the "cheat code" you've been looking for.
