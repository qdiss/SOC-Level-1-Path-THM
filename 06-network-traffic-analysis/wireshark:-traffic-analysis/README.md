# Wireshark: Traffic Analysis - TryHackMe SOC Level 1

- **Module:** Network Traffic Analysis
- **Date completed:** 2026-11-14
- **Room link:** https://tryhackme.com/room/wiresharktrafficanalysis  
- **Path:** SOC Level 1

## Overview

This is the third room of the Wireshark series. After learning the
interface and filters, I used Wireshark to detect real attack patterns
and anomalies in packet captures: network scanning, ARP poisoning,
host and user identification, HTTP and Log4j activity, decrypting
HTTPS traffic, FTP abuse, and cleartext credential hunting. The room
also showed how to turn findings into firewall rules.

## Objectives

- Detect and investigate anomalies at packet level
- Recognise the traffic patterns of common attacks
- Identify hosts and users from network protocols
- Analyse cleartext and encrypted protocols
- Turn findings into actionable results

## Tools / Concepts Covered

- Nmap scan patterns: TCP connect, SYN, and UDP scans
- TCP flags and filters for scan detection
- ARP analysis and ARP poisoning (man-in-the-middle) detection
- Host and user identification with DHCP, NetBIOS (NBNS), and Kerberos
- HTTP analysis: methods, response codes, user-agent anomalies, Log4j exploitation traces
- HTTPS decryption with a TLS key log file
- FTP analysis for brute force and suspicious uploads
- Tools menu: Credentials and Firewall ACL Rules

## Analysis Process

### Step 1 - Recognising Scan Activity

Different Nmap scan types leave different packet patterns. A TCP
connect scan completes the three-way handshake and typically uses
larger window sizes, a SYN scan stops before completing the handshake
and typically uses small window sizes, and a UDP scan shows ICMP
"port unreachable" replies for closed ports while open ports stay
silent. I used TCP flag and ICMP filters to find each pattern in a
capture and to tell which ports were targeted.

### Step 2 - Detecting ARP Poisoning

ARP has no authentication, so attackers can claim another host's IP
address. I looked for the signs: two MAC addresses claiming the same IP
(especially a likely gateway), a single host sending bursts of ARP
requests to many addresses, and Expert Information warnings about
duplicate addresses. Adding MAC addresses as columns showed that HTTP
traffic meant for a victim was being delivered to the attacker's
machine, confirming a man-in-the-middle attack. I documented findings
in a table of observations as I went, which kept the investigation
organised.

### Step 3 - Identifying Hosts and Users

DHCP request packets reveal hostnames, requested addresses, and client
MAC addresses, NBNS shows NetBIOS names and registrations, and
Kerberos packets show account names and domains, with names ending in
`$` indicating machines rather than users. These sources help decide
where an investigation should start and which hosts and users are tied
to suspicious activity.

### Step 4 - HTTP, User Agents, and Log4j

HTTP is cleartext, so it exposes requests, response codes, and
parameters useful for spotting phishing pages, web attacks,
exfiltration, and C2. I reviewed user-agent values for anomalies such
as audit tools, custom strings, subtle spelling differences, and
payloads in the field, and I learned never to whitelist a user agent
just because it looks normal. For Log4j, I searched for the known
cleartext patterns of the exploit and decoded an encoded command to
find what the attacker tried to reach.

### Step 5 - Decrypting HTTPS

TLS hides URLs and data, but the handshake still reveals the endpoints
involved. With a key log file saved during the capture, Wireshark can
decrypt the sessions, which exposes decompressed headers and HTTP/2
details. The key log must be generated during the capture, otherwise
decryption is not possible.

### Step 6 - FTP, Cleartext Credentials, and Firewall Rules

FTP response codes and commands make it easy to spot failed logins,
password spraying, file uploads, and permission changes. The Credentials
tool lists cleartext logins found in supported protocols, which is
faster than reading packets one by one, though manual checks are still
needed. Finally, the Firewall ACL Rules tool generates block or allow
rules for several firewall formats directly from a selected packet.

## Outcome / Classification

Completed the questions on scan detection, ARP poisoning, host and user
identification, HTTP and Log4j analysis, HTTPS decryption, FTP abuse,
credential hunting, and firewall rule generation.

## Key Takeaways

Each attack leaves a recognisable shape in the traffic, and knowing the
normal protocol behaviour is what makes the anomaly visible. The best
workflow was to start with a broad filter for the pattern, then narrow
down to hosts, take notes, and correlate across protocols, as in the
ARP case where the IP view looked normal until MAC addresses were
added. Wireshark is not an IDS, so the results depend on analyst
knowledge, but its tools for credentials and firewall rules make it
possible to move from detection to a concrete action.

## Skills Demonstrated

- Identifying TCP connect, SYN, and UDP scans from packet patterns
- Detecting ARP poisoning and man-in-the-middle activity
- Using DHCP, NBNS, and Kerberos traffic to identify hosts and users
- Spotting user-agent anomalies and Log4j exploitation traces in HTTP traffic
- Decrypting HTTPS traffic with a key log file
- Detecting FTP brute force and suspicious uploads
- Generating firewall rules from packet captures
