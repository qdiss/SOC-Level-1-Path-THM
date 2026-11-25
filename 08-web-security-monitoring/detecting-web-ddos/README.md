# Detecting Web DDoS - TryHackMe SOC Level 1

- **Module:** Web Security Monitoring
- **Date completed:** 2026-11-25
- **Room link:** https://tryhackme.com/room/detectingwebddos
- **Path:** SOC Level 1

## Overview

This room covers application-layer (Layer 7) denial-of-service attacks
against websites and web applications. I learned how DoS and DDoS
attacks work, why attackers carry them out, how they show up in web
server logs, and how defenders detect and mitigate them. I practised on
an access log and then on a larger dataset in Splunk.

## Objectives

- Learn how denial-of-service attacks function
- Understand attacker motives
- See how web logs reveal DoS and DDoS activity
- Practise analysing attacks through log analysis
- Discover detection and mitigation techniques

## Tools / Concepts Covered

- DoS vs. DDoS and botnets
- Attack types: Slowloris, HTTP flood, cache bypass, oversized queries, login and form abuse, faulty input validation abuse
- Attacker motives: financial loss, extortion, hacktivism, distraction, competition, denial of wallet, reputational damage
- Log indicators: high request rate, odd user agents, geographic anomalies, burst timestamps, 5xx errors, logic abuse
- Resource-heavy endpoints that attackers target (login, search, API, registration, checkout)
- Splunk searches, extracted fields, and `timechart`
- Defences: input validation, CAPTCHA and JavaScript challenges, CDN, load balancing, WAF rate limiting

## Analysis Process

### Step 1 - Understanding DoS and DDoS

A DoS attack aims to stop a service from working as intended. A single
machine is limited by its own resources, so attackers use botnets of
compromised devices to generate DDoS traffic at scale. At the
application layer, even a single malformed or oversized request to a
poorly validated search form can cause an outage. The motives range
from financial pressure and extortion to political protest,
distraction from another attack, and damaging a company's reputation.
Real incidents against large media and cloud services showed that even
big organisations are affected.

### Step 2 - Detecting DoS in Logs

I reviewed the indicators that point to an attack: a very high request
rate to one resource, tool-like or unusual user agents, requests from
widely scattered geographic locations, many requests in the same second,
a surge of 5xx errors, and queries designed to overload the server.
Attackers favour expensive endpoints such as login, search, API, and
checkout pages because each request needs database queries or
processing. In a sample access log, I identified the source of the
flood, the page being hammered, and the error that legitimate users
received after the server was overwhelmed. No single indicator is
enough, so I looked for several together.

### Step 3 - Investigating in Splunk

A SIEM makes this far faster than reading raw logs. In Splunk I
searched the web logs, used the extracted fields (URI, client IP,
user agent, status code) to find the most requested page, the most
active sources, the number of IP addresses taking part in the botnet,
and the user agent used by the attack traffic. A `timechart` showed the
sudden spike in requests per second, and I traced which legitimate
visitor first received a server error once the site started failing.

### Step 4 - Prevention and Mitigation

At the application level, secure development and strict input
validation stop attackers from abusing forms with oversized or unusual
input, and CAPTCHA or JavaScript challenges filter out bots. At the
network level, a CDN absorbs traffic, serves cached content from edge
servers, and balances load across servers, while a WAF can apply rules
such as rate limiting a login page. Attackers try to bypass these
protections with tricks like random query parameters that defeat
caching, rotating user agents, and distributing requests across many
regions.

## Outcome / Classification

Identified the attack source, the targeted endpoint, the botnet size,
and the attack's peak, and confirmed the impact on legitimate users in
both the access log and Splunk.

## Key Takeaways

DDoS detection is mostly pattern recognition across fields: the same
URI, a narrow set of user agents, and a sharp spike in time. A
timechart makes an attack obvious in seconds, while the field
breakdowns identify who and what is involved. Mitigation works best in
layers, since a WAF rate limit alone can be bypassed with cache
busting or distributed sources, but combined with a CDN, challenges,
and input validation it makes an attack much more expensive for the
attacker. This room built on the earlier web security rooms by showing
how WAF and CDN controls appear in practice.

## Skills Demonstrated

- Recognising web DoS and DDoS indicators in access logs
- Using Splunk fields, statistics, and `timechart` to identify attack sources and peaks
- Distinguishing DoS attack types and attacker motives
- Explaining how CDNs, WAF rate limiting, and challenges mitigate attacks
- Understanding common bypass techniques used by attackers
