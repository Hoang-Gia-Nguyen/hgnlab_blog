+++
title = "Building Your Very First Homelab: Block Ads With AdGuard Home"
date = 2025-07-15
draft = false
description = "Move from theory to practice with this guide to building your first homelab. I'll show you how I use Docker to run AdGuard Home, a powerful network-wide ad-blocker, and how you can get started with your own hardware."
[taxonomies]
categories = ["Technology"]
tags = ["self-hosting", "homelab", "docker"]

[extra]
cover.image = "images/self-hosting-adguard.png"
cover.alt = "A clean and modern desk setup representing a homelab."
+++

You might have even read my [previous post on the "what" and "why" of self-hosting](@/what-why-self-hosting.md). But there's a big difference between understanding a concept and living it. So, where do you actually start?

It starts with a homelab.

Don't let the name intimidate you. A "homelab" isn't a rack of blinking servers in a chilled basement—at least, not at first. It's simply a computer in your home that's always on, ready to run the services you choose.

By the end of this article, you will have a functional, simple homelab with your first self-hosted application running. No expert knowledge required. Let's build something.

---

### Chapter 1: Choosing Your Hardware (Without Breaking the Bank)

The first rule of homelabbing is to start small and cheap. The goal is to learn and experiment, not to build a data center on day one. You probably already have something that will work perfectly.

#### Option A: The Humble Raspberry Pi
This is the classic entry point for a reason.
*   **Pros:** Sips power, is completely silent, has a massive community for support, and is the perfect size for a beginner's needs.
*   **Cons:** Its processing power is limited, and supply chain issues can sometimes make them hard to find.
*   **Recommendation:** Ideal for running a handful of lightweight services.

#### Option B: The Repurposed Hero (Old Laptops & Desktops)
Before you recycle that old hardware, consider its potential.
*   **Pros:** It's free! And it's almost certainly more powerful than a Raspberry Pi. An old laptop is especially great because it has a built-in battery backup (UPS).
*   **Cons:** It will use more power and take up more space than a Pi.
*   **Recommendation:** A fantastic, cost-effective way to give old hardware a new purpose.

#### Option C: The Compact Powerhouse (Mini PCs)
If you're willing to spend a little money, you can get a dedicated machine.
*   **Pros:** Excellent performance-per-watt, compact, quiet, and modern. Brands like Beelink, Minisforum, or the Intel NUC line are popular choices.
*   **Cons:** They aren't free. Prices can range from $150 to $400 for a capable machine.
*   **Recommendation:** The best choice if you know you want to get serious about self-hosting and want a solid foundation to build on.

For this guide, I'll be using my **Mac Mini M4**. Why? Because it's the machine I already have, and it's on 24/7 anyway. The best server is often the one you don't have to buy. The Apple M4 chip is also incredibly power-efficient, which is a huge plus for an always-on device.

Now, you might think macOS isn't an ideal server operating system, and you'd generally be right. However, this becomes much less of a factor when you embrace containerization. Since nearly all of my self-hosted services run inside Docker containers, the underlying OS doesn't matter nearly as much. Docker provides a consistent, isolated Linux environment for my applications, giving me the best of both worlds: the power and efficiency of Apple hardware and the massive ecosystem of Linux server applications.

The rest of this tutorial will assume you are working within a Linux environment, which in my case is provided by Docker Desktop on macOS. The commands will be the same whether you're on a Mac with Docker, a dedicated Linux machine, or a Raspberry Pi.

---

### Chapter 2: The Brains of the Operation - Choosing an OS

Your hardware needs a brain—an operating system. While I'm using macOS with Docker, the most common and recommended path for a dedicated homelab server is to use a Linux-based OS.

#### The Go-To Choice: A Standard Server OS
*   **What:** **Ubuntu Server** or **Debian**.
*   **Why:** They are the bedrock of the server world. They are incredibly stable, secure, and have more guides, tutorials, and forum posts than you could read in a lifetime. For our purposes, they provide the perfect, no-fuss foundation for running Docker directly.
*   **Installation:** The installation process for these is incredibly well-documented. Your goal is to get the OS installed and ensure you can connect to it from your main computer using SSH.
    *   For Raspberry Pi: Use the official [Raspberry Pi Imager](https://www.raspberrypi.com/software/). It's a fantastic tool that does all the hard work for you.
    *   For other computers: Follow the official [Ubuntu Server installation guide](https://ubuntu.com/tutorials/install-ubuntu-server).

---

### Chapter 3: First Steps - Networking & Docker

With your server up and running, there are two concepts to understand before we can deploy our first application: stable networking and containerization.

#### The Importance of a Static IP Address
Your home router assigns local IP addresses (like `192.168.1.123`) to devices when they connect. By default, this is dynamic, meaning a device's IP address can change every time it restarts. For a laptop or phone, this is perfectly fine.

For a server, however, it's a problem. If you want to reliably connect to your services (like our ad-blocker), you need to know its address. If the address keeps changing, you'd have to find it every time you restart the machine.

Assigning a **static IP address** solves this. It's like giving your server a permanent, reserved parking spot on your network. You're telling your router that this specific computer should *always* have this specific address. This is typically done in your router's administration settings, often under a section called "DHCP Reservations" or "Static Leases."

#### Introducing Docker: Your Best Friend in Self-Hosting
What is Docker? In one sentence: **Docker lets you run applications in clean, isolated packages, so you don't mess up your server's operating system.**

Before Docker, you would install applications directly onto the server. This could lead to conflicts between applications needing different versions of the same dependency—a situation affectionately known as "dependency hell."

Docker solves this by wrapping each application and all its dependencies into a self-contained "container." You can run dozens of containers on the same machine without them ever interfering with each other or with the host OS. It simplifies installation, updates, and removal of applications to a few simple commands.

Getting Docker installed is the next step. The process varies slightly depending on your operating system, so it's best to follow the official documentation.
*   **[Official Docker Installation Guides](https://docs.docker.com/engine/install/)**

---

### Chapter 4: Your First Application - A "Hello World" Moment with AdGuard Home

It's time for the payoff. We're going to install **AdGuard Home**, a network-wide ad and tracker blocker.

*   **Why AdGuard Home?** It provides immediate, tangible value—once it's running, it will block ads on *every device* on your network (phones, laptops, smart TVs). It's lightweight, has a great web interface, and is the perfect introduction to the server/client model.

We'll use `docker-compose` to define the application. This is a simple text file that describes the service we want to run.

1.  Create a new folder for your AdGuard Home configuration: `mkdir adguard && cd adguard`
2.  Create a new file named `docker-compose.yml`: `nano docker-compose.yml`
3.  Paste the following content into the file:

```yaml
version: "3.7"
services:
  adguardhome:
    image: adguard/adguardhome
    container_name: adguardhome
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "3000:3000/tcp"
    volumes:
      - ./workdir:/opt/adguardhome/work
      - ./confdir:/opt/adguardhome/conf
    restart: unless-stopped
```

Let's quickly break this down:
*   `image`: Tells Docker which application to download.
*   `ports`: Maps ports from the server to the Docker container. We map port `53` for DNS queries and port `3000` for the web interface.
*   `volumes`: This is crucial. It maps configuration directories from inside the container to the `workdir` and `confdir` folders on our server. This means your AdGuard settings will persist even if you update or recreate the container.

Now, for the magic command. From inside your `adguard` directory, run:

```bash
docker-compose up -d
```

Docker will now download the AdGuard Home image and start it as a background service. You can check that it's running with `docker ps`.

#### Configuration & Router Setup

Once the container is running, there are two final configuration stages: setting up the application itself, and then telling your network to use it.

1.  **AdGuard Home First-Time Setup:** Open a web browser and navigate to `http://<your-server-ip>:3000`. You'll be greeted by the AdGuard Home setup wizard. It's very straightforward; follow the on-screen instructions to set an admin username and password.

2.  **Configure Your Router:** This is the most important step. To get network-wide ad blocking, you need to tell your router to use your new AdGuard Home server as its DNS resolver. The process is slightly different for every router, but you generally need to:
    *   Log in to your router's administration page (usually something like `192.168.1.1`).
    *   Find the **DNS** settings. This is often in the **DHCP** or **LAN** section.
    *   Change the primary (and if possible, secondary) DNS server to the static IP address you assigned to your homelab server. In my case 192.168.1.10 is my server static IP.
    *   Save the settings. Your router may need to reboot.

![Setting Primary DNS to your server IP](/images/screenshot_setdns.png)

Once your router is using AdGuard Home for DNS, every device that connects to your network will automatically have its ads and trackers blocked.

And just like that... silence. The ads are gone. Browse a few websites on your phone or computer; you'll notice the difference immediately. You're not just using the internet, you're starting to *control* it.

![AdGuard Home Interface](/images/screenshot_adguardhome.png)

---

### Conclusion: You've Built a Homelab!

Take a moment to appreciate what you've accomplished. You took a piece of hardware, installed a server operating system, gave it a stable address on your network, and deployed a real-world application that improves your daily internet experience. You have a homelab.

The journey doesn't end here. This is your foundation. Now that you have this setup, you can explore hosting your own:
*   **Password Manager** with Vaultwarden
*   **File Sync Service** (like your own personal Dropbox) with Syncthing
*   **Media Server** with Jellyfin

The possibilities are endless. So, what will you host first? Let me know in the comments below!
