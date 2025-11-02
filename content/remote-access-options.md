+++
title = "Securely Access Your Self-Hosted Services From Anywhere"
date = 2025-11-02
draft = false
description = "A guide to understanding and choosing the right remote access solution for your self-hosted applications, from reverse proxies to zero-trust tunnels."
[taxonomies]
categories = ["Self-hosting"]
[extra]
cover.image = "images/remote-access-cover.png"
cover.alt = "A person accessing a server from a laptop"
+++

When you're self-hosting, you'll eventually want to access your services from outside your home network. Whether it's sharing a photo gallery with family, accessing your files on the go, or managing your services remotely, you need a way to connect to your home lab from the internet. This post will explore two popular methods for achieving this: reverse proxies and zero-trust tunnels.

## Reverse Proxies

A reverse proxy is a server that sits in front of your web servers and forwards client requests to those servers. It's like a receptionist for your network, directing traffic to the right place.

### Pros

*   **Centralized Management:** You can manage all your services from a single point.
*   **SSL Termination:** Easily manage SSL certificates for all your services in one place.
*   **Load Balancing:** Distribute traffic across multiple servers to improve performance and reliability.
*   **Security:** Hide the IP addresses of your backend servers and provide an additional layer of defense.

### Cons

*   **Requires a Public IP Address:** You need a static public IP address or a dynamic DNS service.
*   **Port Forwarding:** You need to open ports on your router, which can be a security risk if not done correctly.
*   **More Complex Setup:** Can be more involved to set up and configure compared to tunneling services.

### Popular Reverse Proxies

*   [**Nginx Proxy Manager**](https://nginxproxymanager.com/): A user-friendly dashboard for managing Nginx reverse proxies, with free SSL via Let's Encrypt.
*   [**Caddy**](https://caddyserver.com/): A modern web server with automatic HTTPS that is easy to configure.
*   [**Traefik**](https://traefik.io/traefik/): A modern reverse proxy and load balancer that integrates seamlessly with Docker and other container orchestrators.
*   [**HAProxy**](https://www.haproxy.org/): A high-performance reverse proxy and load balancer for high-traffic websites.

## Zero-Trust Tunnels

Zero-trust is a security model that assumes no user or device is trusted by default. A zero-trust tunnel creates a secure, outbound-only connection from your server to a service provider's network. You then access your service through the provider's network, which handles authentication and authorization.

### Pros

*   **No Public IP or Port Forwarding:** You don't need to expose any ports on your router, making it a more secure option.
*   **Easy to Set Up:** Generally easier to set up and configure than a reverse proxy.
*   **Access Control:** Granular control over who can access your services.

### Cons

*   **Reliance on a Third-Party Service:** Your access depends on the availability of the tunneling service.
*   **Potential for Slower Speeds:** Your traffic is routed through the service provider's network, which can sometimes result in slower speeds.
*   **Cost:** Some services may have costs associated with them, especially for more advanced features.

### Popular Zero-Trust Tunneling Services

*   [**Cloudflare Tunnel**](https://www.cloudflare.com/products/tunnel/): A free and easy-to-use service that creates a secure tunnel from your server to the Cloudflare network.
*   [**Tailscale**](https://tailscale.com/): A peer-to-peer VPN that creates a secure network between your devices.
*   [**Zrok**](https://zrok.io/): An open-source, self-hostable tunneling service with a focus on zero-trust principles.
*   [**frp (Fast Reverse Proxy)**](https://github.com/fatedier/frp): A production-ready reverse proxy and tunneling solution that you can self-host.

Which option is right for you depends on your needs and technical expertise. If you're looking for a simple and secure way to access your services, a zero-trust tunnel is a great option. If you want more control and flexibility, a reverse proxy might be a better fit.
