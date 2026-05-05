---
layout: post
title: "Automating DNS Record Changes using Cloudflare API"
---


In my homelab, I run a WireGuard server that gives me secure access to my private home network from anywhere. Like most setups, I prefer using a readable domain name instead of remembering or typing a raw IP address. On top of that, residential ISP connections don’t always guarantee a static IP, so I can’t assume my home address will stay the same over time.

To solve this, I built a simple Bash script that runs as a cron job. It periodically checks my WAN IP address and compares it against the DNS record in Cloudflare. If it detects a change, it automatically updates the record using the Cloudflare AP  ensuring my domain always points to the correct IP without any manual intervention.

