---
layout: single
title: "Automating DNS Record Changes using Cloudflare API"
---


In my homelab, I run a WireGuard server that gives me secure access to my private home network from anywhere. Like most setups, I prefer using a readable domain name instead of remembering or typing a raw IP address. On top of that, residential ISP connections don’t always guarantee a static IP, so I can’t assume my home address will stay the same over time.

To solve this, I built a simple Bash script that runs as a cron job. It periodically checks my WAN IP address and compares it against the DNS record in Cloudflare. If it detects a change, it automatically updates the record using the Cloudflare AP  ensuring my domain always points to the correct IP without any manual intervention.



# Setting up your API token in Cloudflare

To create your first API token, you will want to navigate to [https://dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens). Selecting "Create API Token" will provide you with the option to create a custom token or use a template. For this script, I created a custom token with the following permissions. I've allowed the token to include all account and zone resources. 


# The Script

This script acts as a simple dynamic DNS updater for a Cloudflare-hosted domain. It first retrieves your current public IPv4 address by querying ifconfig.me, then fetches the existing A record value from Cloudflare for a specific DNS zone using the Cloudflare API. It compares the two values, and if they differ, it updates the DNS record via a PUT request so the domain points to your current IP. If the IP hasn’t changed, it exits without making any updates.

<img src="{{site.url}} {{ site.baseurl}}/assets/images/blog-caps/cloudflare-template.png" alt="">



```bash
#!/bin/bash

ip=$(curl -s -4 --max-time 5 ifconfig.me)

cfIP=$(curl -s "https://api.cloudflare.com/client/v4/zones/$ZONEID/dns_records/" \
  -H "Authorization: Bearer $APITOKEN" \
  | jq -r '.result.content')

echo "My public IP is $ip"
echo "CF IP is $cfIP"


if [ "$cfIP" != "$ip" ]; then
        curl https://api.cloudflare.com/client/v4/zones/$ZONEID/dns_records/$RECORDID \
    -X PUT \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer $APITOKEN" \
        -d "{
         \"name\": \"vpn.wabbledee.com\",
         \"ttl\": 3600,
         \"type\": \"A\",
         \"comment\": \"\",
         \"content\": \"$ip\",
         \"private_routing\": false,
         \"proxied\": false
        }"
        else
        echo "no change"
        exit 0
        fi
```
