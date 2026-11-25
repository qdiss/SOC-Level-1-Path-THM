# Detecting Web Attacks - TryHackMe SOC Level 1

- **Module:** Web Security Monitoring
- **Date completed:** 2026-11-25
- **Room link:** https://tryhackme.com/room/detectingwebattacks/
- **Path:** SOC Level 1

## Overview

This room teaches how to detect common web attacks from a SOC
perspective. It covers client-side and server-side attack types, then
shows how the same attack sequence looks in web server access logs and
in network traffic, and finishes with Web Application Firewall (WAF)
rules as a mitigation. I investigated a simulated breach of an online
banking site using both an access log and a packet capture.

## Objectives

- Learn common client-side and server-side attack types
- Understand the benefits and limitations of log-based detection
- Explore network traffic based detection
- Understand how and why WAFs are used
- Practise identifying common web attacks with these methods

## Tools / Concepts Covered

- Client-side attacks: cross-site scripting (XSS), cross-site request forgery (CSRF), clickjacking
- Server-side attacks: brute force, SQL injection, command injection
- Web server access log fields and the indicators in each
- Limitations of access logs (no POST body, optional query strings)
- Wireshark HTTP filters and following HTTP streams
- WAF rule types: attack patterns, deny lists, custom rules, rate limiting, and challenge-response
- Threat intelligence feeds and built-in rule sets in WAFs

## Analysis Process

### Step 1 - Client-Side vs. Server-Side Attacks

Client-side attacks abuse the user's behaviour or browser, so they
leave little or no trace in server logs or network traffic, which
makes them hard for a SOC to detect without endpoint or browser-side
controls. Server-side attacks target the server, application code, or
backend and are more visible, because every request is processed by
the server and travels across the network. I reviewed XSS, CSRF, and
clickjacking on the client side and brute force, SQL injection, and
command injection on the server side, with real breaches as examples.

### Step 2 - Detecting Attacks in Access Logs

Each access log field can be a clue: the client IP (known malicious or
unexpected geography), the timestamp and requested page (odd hours or
bursts), the status code (many 404 responses suggest scanning, and a
change in response after many failures suggests a successful login),
the response size, the referrer, and the user agent (attack tools or
outdated browsers). An attack sequence showed three stages in the log:
a directory fuzz to find pages, a brute force against a login form
with repeated POST requests ending in a different response code, and
SQL injection payloads against a search or form field. The limitation
is that access logs usually do not record POST bodies, so I could see
that login attempts happened but not the credentials used.

### Step 3 - Confirming in Network Traffic

A packet capture fills the gaps in the logs. For unencrypted HTTP I
could see the full requests, including the submitted credentials, the
SQL injection payloads, and the data returned to the attacker, which
showed what was actually stolen. Filtering with the `http` filter and
following the HTTP stream rebuilt each request and response. The
limitation is encrypted traffic such as HTTPS, which hides the
payload without the keys.

### Step 4 - Web Application Firewalls

A WAF inspects full HTTP requests, and can decrypt TLS before filtering
them, then allows, blocks, or challenges them according to rules.
Rule categories include blocking known attack patterns (for example
malicious user agents), denying known bad sources, custom rules for
the application, and rate limiting. A CAPTCHA challenge can be used
instead of a block where legitimate users might be affected, and many
WAFs add built-in rules for the OWASP Top 10 and threat intelligence
feeds. I created a custom rule that blocks requests based on a
user-agent match.

## Outcome / Classification

Reconstructed the attack chain (directory fuzzing, brute force, and SQL
injection) from the access log, then used the packet capture to
identify the compromised account and the data taken, and wrote a
custom WAF rule to block the tooling.

## Key Takeaways

Logs and packet captures answer different questions: logs show the
pattern and timeline of an attack across many requests, while packets
show the content. Using both gave me a complete picture that neither
gave alone. I also learned to be realistic about visibility: a SOC
can detect most server-side attacks but has little visibility into
client-side attacks, and encrypted traffic needs decryption to be
inspected. For mitigation, simple WAF rules such as blocking known
tooling and rate limiting login attempts can stop a lot of automated
attacks with little effort.

## Skills Demonstrated

- Recognising directory fuzzing, brute force, and SQL injection patterns in web access logs
- Using Wireshark to confirm web attacks and recover the content of requests
- Explaining what logs can and cannot show
- Writing basic WAF rules to block malicious user agents
- Correlating log and network evidence to determine the scope of a breach
