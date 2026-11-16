# Network Discovery Detection - TryHackMe SOC Level 1

**Module:** Network Security  
**Date completed:** 2026-11-16  
**Room link:** https://tryhackme.com/room/networkdiscoverydetection  
**Path:** SOC Level 1

## Overview

This room covers network discovery: how attackers map a target network, how defenders do the same for legitimate reasons, and how a SOC analyst tells them apart in the logs. It explains external versus internal scanning and their severity, the difference between horizontal and vertical port scans, and how ping sweeps, TCP SYN scans, and UDP scans work. I practised on firewall logs exported from a SIEM as CSV files and then on the same data in Kibana, in a lab virtual machine.

## Objectives

- Understand what network discovery is
- Understand why attackers perform network discovery
- Learn the different types of network discovery
- Learn how network discovery techniques work and how to detect them

## Tools / Concepts Covered

- Attack surface and the information attackers look for: reachable assets, IP addresses, ports, OS, services, and service versions
- Defender discovery goals: asset inventory, closing unnecessary ports and services, patching vulnerabilities
- Telling good discovery from bad: allowlisting known scanners, threat intelligence to flag or raise severity, generic scan-behaviour detections
- External scanning (Reconnaissance phase) and internal scanning (Discovery phase) in MITRE ATT&CK
- Horizontal scanning: same destination port, many destination IPs (example: WannaCry looking for SMB on port 445)
- Vertical scanning: one destination IP, many destination ports
- Scan mechanics: ping sweep (ICMP), TCP SYN scan, UDP scan
- Zeek connection logs and the zeek.conn.conn_state field
- Command line log analysis with head and cut on SIEM CSV exports
- Kibana: Discover, the All logs data view, adding columns, and filtering values in and out

## Analysis Process

### Step 1 - Attackers, Defenders, and Discovery

I started with why discovery matters. Attackers scan to find what is reachable, which IPs, ports, OS, and services are running, and whether any service version is vulnerable. Defenders scan to keep an inventory, close unneeded ports, and patch, which reduces the attack surface. Because both sides scan, and so do researchers, search engines, and crawlers, the SOC has to separate good from bad discovery. Common approaches are allowlisting known internal and benign scanners, using threat intelligence to flag scanning from known malicious sources or to raise alert severity, and adding generic scan-behaviour detections.

### Step 2 - External vs Internal Scanning

External scanning has an external source IP and an organisation destination IP. The attacker has no foothold yet and is in the Reconnaissance phase, so it is low severity, and the usual response is blocking the IP on the perimeter firewall, knowing the attacker can return from another IP. Internal scanning has private IPs as both source and destination, which means the attacker is already inside and in the Discovery phase, preparing for lateral movement. That is a high-severity alert: after ruling out authorised activity, the analyst escalates, starts incident response, and does a deeper investigation and root cause analysis rather than just blocking an IP.

In the lab I went to /home/ubuntu/Downloads/logs, previewed the exported CSV files with head, and used cut with a comma delimiter to pull out columns, remembering that the timestamp contains a comma and shifts the column numbers. From the logs I identified the file with internal scanning, counted the entries for the internal scanning IP, and found the external IP doing external scanning.

### Step 3 - Horizontal and Vertical Port Scans

A horizontal scan hits one port across many IPs to find hosts exposing that service. In the logs it shows as one source IP, one destination port, and many destination IPs. A vertical scan hits one host across many ports to footprint its services. In the logs it shows as one source IP, one destination IP, and many destination ports. Attackers also mix the two. In the log files I identified the scanned IP range, the host that was vertically scanned, and the ports checked on a host where only a few common services were probed.

### Step 4 - How Scans Work

- Ping sweep: ICMP echo requests to find online hosts. Easy to defeat because many organisations block ICMP.
- TCP SYN scan: send a SYN, and a SYN-ACK back means the host is online and the port is open. It is stealthy and blends into normal traffic.
- UDP scan: send an empty UDP packet. An ICMP port unreachable reply means the port is closed and the host is up, silence until timeout is marked open but is not clear evidence, and a UDP reply is real evidence of an open port. It is slow and unreliable.

Organisations also run their own internal scans, so the SOC should know the source IPs, scan types, and schedules and exclude them from detections to reduce noise.

### Step 5 - Identifying Scan Types in Kibana

I logged into Kibana from the browser in the VM, opened Discover, chose the All logs data view with the entire time range, and added fields as columns. Using filters on field values, I found the source IP performing a ping sweep across a whole subnet, used the zeek.conn.conn_state value to work out which scan type 203.0.113.25 ran against 192.168.230.145, and checked the logs for any UDP scanning.

## Outcome / Classification

Completed all questions on internal and external scanning, horizontal and vertical scans, and scan type identification. I classified scanning activity by direction (external as reconnaissance, internal as discovery with a possible foothold) and by pattern (horizontal, vertical, ping sweep, SYN, UDP).

## Key Takeaways

The direction of a scan decides its severity: external-to-internal is early reconnaissance, internal-to-internal means the attacker is already inside. The log pattern tells me the scan type: one port across many IPs is horizontal, many ports on one IP is vertical. Raw SIEM exports are messy, so being comfortable with head and cut and with Kibana filtering makes triage much faster. Good detection also needs context, such as allowlisted scanners and threat intelligence, because defenders and researchers scan too. This room builds on Network Security Essentials.

## Skills Demonstrated

- Distinguishing external reconnaissance from internal discovery and judging severity
- Identifying horizontal and vertical scans from log patterns
- Explaining ping sweeps, TCP SYN scans, and UDP scans
- Reading Zeek connection states to infer scan type
- Analysing SIEM CSV exports with head and cut
- Searching, filtering, and adding columns in Kibana Discover
