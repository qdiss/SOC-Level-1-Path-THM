# Network Security Essentials - TryHackMe SOC Level 1

**Module:** Network Security  
**Date completed:** 2026-11-16  
**Room link:** https://tryhackme.com/room/networksecurityessentials  
**Path:** SOC Level 1

## Overview

This room introduces network security from a defensive perspective. It covers how a network is built, why the network perimeter matters, which log sources give an analyst visibility, and how to read firewall, WAF, VPN, and IDS logs to spot early-stage attacks. I practised on perimeter logs in a lab virtual machine and finished with an incident investigation for a fictional company, Initech Corp, where I had to work out what techniques the adversary used and whether they breached the perimeter.

## Objectives

- Understand what a network is and identify its key components
- Explore the concept of the network perimeter and its importance
- Identify the key perimeter threats
- Examine the firewall logs to monitor normal and suspicious logs

## Tools / Concepts Covered

- Network components: user workstations, file and database servers, application servers (web, email, VPN), Active Directory, routers and switches, firewalls
- Network perimeter: firewalls, routers and gateways, DMZ, remote access gateways and VPNs
- Host-centric logs: operating system logs, application logs, security tool logs (antivirus, EDR, HIDS)
- Network-centric logs: firewalls, IDS/IPS, routers and switches (flow data), web proxies, VPN
- Perimeter attacks: port scanning, web attacks (SQL injection, XSS, directory traversal), VPN brute force, lateral movement, C2 beaconing, data exfiltration
- WAF logs and IDS alerts
- Log analysis on the command line with grep, cut, sort, and uniq
- Log analysis in Splunk (logs ingested in index="network_logs", Search & Reporting)

## Analysis Process

### Step 1 - Network Components

I started with the building blocks of a small enterprise network and why each matters for security. Workstations are the most common entry point (phishing, malicious downloads). File and database servers hold the most valuable data and are targeted by ransomware and exfiltration. Application servers are externally facing, so attackers scan them constantly. Active Directory controls all accounts, so one compromised domain admin can bring down the whole network. Routers and switches can be used for man-in-the-middle attacks and hidden channels if compromised, and firewalls log every connection attempt, which makes them an early indicator of attacks.

### Step 2 - The Network Perimeter

The perimeter is the boundary between the trusted internal network and the untrusted Internet, and it is the first line of defense. It includes firewalls, routers and gateways, a DMZ for public-facing servers, and VPN gateways for remote access. A weak or misconfigured perimeter exposes services such as RDP, SSH, or SMB and allows scanning, brute force, malware infiltration, and data exfiltration. As an analyst I review firewall logs for blocked and allowed connections, identify scanning and brute-force attempts, and flag unusual outbound traffic.

### Step 3 - Network Visibility

You can't defend what you can't see, so visibility comes from two kinds of logs. Host-centric logs show what happened inside a machine. Network-centric logs show who talked to whom: source and destination IPs, ports, protocols, and the action taken. Host logs tell you what happened inside the room, and network logs tell you who entered and left the building, so I need both to build a complete incident timeline.

### Step 4 - Monitoring the Perimeter in Action

I analysed three scenarios from logs in the Perimeter_logs folder.

- Port scan: one external IP hit many ports on the same internal machine in quick succession, and the firewall blocked most of them.
- Web attack: filtering the WAF logs on action=BLOCK showed SQL injection, XSS, and directory traversal attempts coming from one source IP.
- VPN brute force: grouping the VPN logs by source IP showed one IP responsible for a flood of failed logins against common usernames, while the scattered successful logins from other IPs were normal.

The patterns to remember: one source to many destinations is scanning, one source to one destination is brute forcing, and traffic at perfect regular intervals is malware beaconing.

### Step 5 - Initech Corp Investigation

For the final challenge I reviewed one month of firewall.log, ids_alerts.log, and vpn_auth.log, using command-line tools and Splunk. I followed the attack through its stages:

- Reconnaissance: counted blocked firewall requests per source IP and found one external IP responsible for the most blocks, probing ports such as 21, 22, 23, 445, and 3389.
- Initial access: that same IP was later allowed through the firewall on several ports, so I pivoted to the VPN logs.
- Credential access: the VPN logs showed repeated failed logins against a service account, followed by a successful login and an assigned internal IP.
- Lateral movement: from the assigned internal IP, the firewall and IDS logs showed scanning and exploitation attempts against internal hosts on SSH, RDP, and SMB (445), with IDS alerts for possible MS-SMB lateral movement.
- C2 beaconing: IDS alerts for possible C2 beaconing at regular intervals to port 4444 on an external IP.
- Data exfiltration: IDS alerts for large HTTP POST uploads from an internal host to an external IP over ports 80 and 8080.

## Outcome / Classification

Completed the task questions and the Initech Corp investigation. I concluded that the perimeter was breached: the adversary scanned the network, brute-forced a VPN service account to get an internal IP, moved laterally over SMB, established C2 beaconing, and attempted data exfiltration.

## Key Takeaways

Visibility needs both host and network logs, and the real skill is separating normal traffic from suspicious patterns. An IDS alert that explains why something was flagged is more useful than a plain firewall block, and context decides what is normal. A single attack leaves traces across firewall, VPN, and IDS logs, so pivoting from one suspicious IP to the other logs is what reveals the full chain. Monitoring the perimeter is the first step in catching attackers before they move deeper into the network.

## Skills Demonstrated

- Identifying network components and explaining their security importance
- Explaining the network perimeter and its common components
- Distinguishing host-centric and network-centric log sources
- Reading firewall, WAF, VPN, and IDS logs
- Recognising port scans, web attacks, brute force, lateral movement, C2 beaconing, and data exfiltration patterns
- Investigating logs with grep, cut, sort, and uniq, and with Splunk
- Correlating events across multiple log sources to reconstruct an attack timeline
