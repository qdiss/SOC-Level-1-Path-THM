# Web Security Essentials - TryHackMe SOC Level 1

- **Module:** Web Security Monitoring
- **Date completed:** 2026-11-25
- **Room link:** https://tryhackme.com/room/websecurityessentials
- **Path:** SOC Level 1

## Overview

This is the introductory room of the Web Security Monitoring module. It
explains how the web works, why web applications are such common
targets, and which protections exist for the application, the web
server, and the host machine. It ends with a hands-on exercise where I
hardened a new web application before its launch.

## Objectives

- Understand the shift from desktop applications to web applications
- Learn why web applications are common targets for attackers
- Explore web infrastructure and the tools used to protect it
- Practise applying security measures to harden a new web application

## Tools / Concepts Covered

- The request-response cycle and the three components of a web service: application, web server, and host machine
- Common web servers (Apache, Nginx, IIS)
- Risks for web application owners and users, with real-world breach examples
- Web access logs and the fields that matter for investigations
- Content Delivery Networks (CDN): IP masking, DDoS absorption, enforced HTTPS, integrated WAF
- Web Application Firewalls (cloud-based, host-based, network-based) and their detection methods
- Antivirus as a host-level layer in defence in depth
- Secure coding, input validation, access control, least privilege, system hardening, patch management, and strong authentication

## Analysis Process

### Step 1 - Why the Web Is a Target

Web applications are always online, reachable by anyone, and often
connected to databases and internal infrastructure. That makes them a
frequent first stage in a larger attack. The room contrasted the
responsibilities of owners (24/7 security, protecting users' data,
keeping up with new threats) with the risks for users (breached
accounts, identity theft, loss of privacy), and used major breaches
caused by an unpatched web server vulnerability and a misconfigured
firewall as examples.

### Step 2 - How the Web Works

A browser sends a request, the web server processes it and returns a
response. Attackers abuse this cycle by flooding servers, bypassing
access controls, or tricking the server into running harmful commands.
Each of the three components (application, web server, host machine)
needs its own protection, so I reviewed what protects each one.

### Step 3 - Protections per Layer

For the application: secure coding, input validation and
sanitisation, and role-based access control. For the web server:
logging, a WAF, and a CDN. For the host machine: least privilege,
system hardening (disabling unneeded services and closing unused
ports), and antivirus. Strong authentication and patch management
apply to all three.

### Step 4 - Logs, CDN, WAF, and Antivirus

Access logs record the client IP, timestamp, requested resource,
response status, and user agent for each request, which lets an analyst
reconstruct a sequence of events such as a visit, a login, and an
account page. A CDN hides the origin server, absorbs large traffic
volumes, enforces HTTPS, and often includes a WAF. A WAF inspects
HTTP requests using signature-based, heuristic, behavioural, and
IP-reputation or location-based detection. Antivirus mainly protects
the host from known malicious files, such as uploaded web shells, but
is only one layer of a broader strategy.

### Step 5 - Practical Hardening Exercise

In the final exercise I secured a new site by applying best practices
to each of the three layers: the web application, the web server, and
the host machine.

## Outcome / Classification

Completed the concept questions and the hardening exercise for all
three layers.

## Key Takeaways

Security for a web service is layered: no single control protects the
application, the server, and the host at once. As a SOC analyst, the
most relevant parts for me are the access logs, which are the main
evidence source for web attacks, and the WAF and CDN, whose logs show
which requests were blocked. I also noted that detection methods
differ in what they catch: signatures catch known tools, heuristics
and behaviour catch unusual patterns, and reputation filtering catches
suspicious sources. This prepares me for the later rooms where I
analyse web attacks in logs.

## Skills Demonstrated

- Explaining the web request-response cycle and the three layers that need protection
- Matching security controls (WAF, CDN, antivirus, hardening, logging) to the right layer
- Reading the key fields of web access logs
- Understanding how WAF detection methods work
- Applying baseline hardening to a web application, server, and host
