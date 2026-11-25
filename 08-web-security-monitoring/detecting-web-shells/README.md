# Web Shell Detection - TryHackMe SOC Level 1

- **Module:** Web Security Monitoring
- **Date completed:** 2026-11-25
- **Room link:** https://tryhackme.com/room/detectingwebshells
- **Path:** SOC Level 1

## Overview

This room explains what web shells are and how to detect them. A web
shell is a malicious script uploaded to a web server that lets an
attacker run commands remotely, often as an initial access method and
as a persistence mechanism. I reviewed how web shells work, interacted
with one in a lab, and then investigated a compromised WordPress site
using web server logs, file system checks, network traffic indicators,
and Linux audit data.

## Objectives

- Understand what web shells are and how attackers use them
- Detect web shell activity through log, file system, and network analysis
- Understand common tooling used in web shell detection

## Tools / Concepts Covered

- Web shell deployment through file upload vulnerabilities and misconfigurations
- Real-world examples: Exchange server compromises by an APT group and a ransomware group
- MITRE ATT&CK mapping (initial access and persistence)
- Command execution functions abused in PHP web shells
- Apache and Nginx access logs, HTTP methods, user agents, query strings, and missing referrers
- Auditd rules and ausearch, and correlation of web logs with audit events
- File system analysis with find and grep, suspicious file names and double extensions
- Wireshark HTTP filters for web shell upload and interaction
- SIEM platforms for centralised log correlation

## Analysis Process

### Step 1 - How Web Shells Work

A web shell abuses legitimate server-side functions that run system
commands, for example the command execution functions in PHP. A simple
one reads a parameter from the URL, runs it, and prints the output,
while advanced ones have password protection and file managers.
Uploading one requires an upload flaw, a misconfiguration, or prior
access, and attackers can then perform reconnaissance, privilege
escalation, lateral movement, and data exfiltration through it. I
interacted with a shell in the lab through a browser and curl to see
how commands are sent and executed with URL encoding.

### Step 2 - Detecting in Web Server Logs

Indicators in access logs include repeated GET requests probing for an
upload location, POST requests to upload endpoints followed by
repeated requests to the same new file, unusual methods such as PUT
or DELETE, altered, outdated, or tool-like user agents, external IP
addresses on an internal site, long or encoded query strings with
parameters like `cmd`, and missing referrers. Changes in response
codes, such as many 404 responses followed by a 200, help trace the
attacker's path. In the lab I filtered the Apache access log with
`grep` to profile the attacker's IP address, the directory and upload
form they found, the web shell file, and the commands they ran.

### Step 3 - Auditd and Correlation

Auditd records system events based on rules, and `ausearch` can
retrieve events that match a rule key. Correlating a suspicious POST
request in the web log with an audit event for file creation or
program execution confirms that a file was written or a command was
run, and by which user or process. A SIEM makes this correlation
easier with centralised collection and targeted queries.

### Step 4 - File System and Network Analysis

On the file system, web shells appear in web roots and upload
directories, temporary directories, or modified existing files, often
with random names or double extensions. `find` can list scripts
created or changed in a given time window, and `grep` can search for
dangerous functions such as `eval(` inside web content. Some platforms
store content in a database, so a malicious payload may not appear in
file searches. In packet captures I looked for the same indicators in
requests, such as unusual methods, encoded payloads, code or commands
in request bodies, and web server processes starting command-line
tools, using Wireshark HTTP filters.

## Outcome / Classification

Identified the attacker's IP address, the web shell upload path, the
shell's file, and the commands run through it, and found the second
file the attacker downloaded onto the server, then inspected the web
shell's code.

## Key Takeaways

Web shell detection works best by combining sources: web logs show the
request pattern, auditd confirms what the server actually did, the
file system shows what was left behind, and packet captures show the
content. No single indicator is conclusive, since a missing referrer
or a `curl` user agent can be benign, so I need to look for a cluster
of indicators and a coherent timeline. It also reinforced a defensive
lesson: most web shells arrive through weak upload validation, so
preventing unrestricted file uploads removes the most common route.

## Skills Demonstrated

- Recognising web shell upload and interaction patterns in Apache access logs
- Using grep and find to hunt for suspicious requests, files, and functions
- Correlating web logs with auditd events
- Spotting web shell activity in network traffic with Wireshark filters
- Mapping web shell behaviour to MITRE ATT&CK techniques
