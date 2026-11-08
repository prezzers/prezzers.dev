---
title: 'I turned my old gaming laptop into a homelab in a weekend'
description: 'Proxmox, Docker, ad blocking, Coolify and a Cloudflare Tunnel on an MSI laptop, plus the bugs that taught me the most.'
pubDate: '2026-11-08'
---

I'd been watching homelab reels for a while: neat little racks, glowing switches, dashboards everywhere. Then I worked out what it would cost to copy one (well over £1,000) and looked at the old MSI GP65 gaming laptop I already had.

So I wiped it and turned it into a server instead. Two days later it was blocking ads for all my devices, hosting my own app on my own domain, and deploying it automatically every time I push to GitHub. Total spend: a USB stick and a domain name.

## What it runs

- **Proxmox VE** on the laptop itself, which splits it into virtual machines.
- A **Docker VM** for home services: AdGuard Home for ad blocking, Uptime Kuma for monitoring, Portainer, and a Homepage dashboard.
- A **Coolify VM**, my self-hosted alternative to Vercel. It builds and deploys my Next.js app, CoreDump, straight from GitHub.
- A **Cloudflare Tunnel**, so the public can reach my apps at `*.prezzers.dev` without me opening a single port on my router.
- **Tailscale**, so I can reach everything privately from my phone, and get ad blocking even on mobile data.

## The bugs that taught me the most

**The server that lived on the wrong network.** The Proxmox installer couldn't see my router, so it quietly gave itself the address `192.168.100.2`, on a completely different network from everything else in my house. The fix was editing three config files by hand, which taught me more about IP addresses and gateways than any tutorial had.

**The redirect loop.** My first test site wouldn't load. Running `curl -I` against it showed a `307` redirect to HTTPS, every single time. Cloudflare was handling HTTPS and talking to my server over plain HTTP, and my server kept sending it back to HTTPS. Round and round. Turning off the redirect fixed it instantly. Finding it was the hard part.

**The monitor that couldn't find anything.** Uptime Kuma kept saying my app was down with `ENOTFOUND`. The app was fine. The monitoring container just couldn't look up domain names through my VPN's DNS. Giving it its own DNS servers fixed it, and taught me a good rule: a monitor shouldn't depend on the things it's monitoring.

## Was it worth it?

Financially, barely. It saves me a little on hosting, and the electricity eats most of that. But in one weekend I got hands-on with hypervisors, DNS, reverse proxies, tunnels, CI/CD, monitoring and backups, and I broke and fixed each one at least once.

This site is running on it right now.

*The whole setup is documented in my [homelab repo on GitHub](https://github.com/prezzers/homelab).*
