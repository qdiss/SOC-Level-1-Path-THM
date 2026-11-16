# Data Exfiltration Detection - TryHackMe SOC Level 1

- **Module:** Network Security Monitoring
- **Date completed:** 2026-11-16
- **Room link:** https://tryhackme.com/room/dataexfildetection
- **Path:** SOC Level 1

## Overview

This room explains how attackers steal data from a network and how a
SOC analyst can detect it. I covered the common exfiltration channels
(DNS, HTTP, FTP, and ICMP) and investigated each one using packet
captures in Wireshark and logs in Splunk, correlating network and log
evidence to identify the compromised hosts and the data that left the
network.

## Objectives

- Understand common methods used for data exfiltration
- Detect exfiltration attempts using network traffic analysis
- Identify signs of exfiltration on endpoints
- Correlate logs in a SIEM to find hidden exfiltration channels

## Tools / Concepts Covered

- Reasons for exfiltration: financial gain, espionage, extortion, sabotage, reconnaissance
- Phases around exfiltration: discovery and collection, staging and compression, transport, C2 coordination
- Network-based, host-based, cloud, covert/encoding, and insider exfiltration techniques
- DNS tunnelling, HTTP POST uploads, FTP transfers, and ICMP tunnelling
- Wireshark display filters for each protocol
- Splunk searches on DNS and HTTP logs (`stats`, `sort`, `where`, field filters)
- Indicators of attack and triage signals (source host, destination, volume, process, supporting evidence)

## Analysis Process

### Step 1 - Understanding Exfiltration

Exfiltration is the unauthorised transfer of data to a destination the
attacker controls, whether deliberate (an insider) or through malware
and stolen credentials. Attackers usually locate the data, stage it
by compressing or encoding it, transfer it over the network or another
medium, and coordinate over C2. Effective detection means correlating
host, network, and cloud telemetry to answer who accessed the data,
what was sent, how it was staged, and where it went. Real-world
groups use techniques such as encrypted HTTPS channels, HTTP POST
requests to C2 servers, cloud-to-cloud transfer, and combining theft
with ransomware for double extortion.

### Step 2 - DNS Exfiltration

DNS is almost always allowed outbound, so attackers encode data in
query names or responses. The indicators are many queries to one
external domain, unusually long or high-entropy subdomains, rare record
types such as TXT, queries without responses, and regular intervals.
In Wireshark I filtered DNS requests without responses and by frame
length, then isolated the domain receiving them. In Splunk I counted
queries per source IP and per query name, and filtered by query length
to find which hosts were sending chunks of data and how many requests
were involved.

### Step 3 - HTTP Exfiltration

HTTP blends into normal web traffic, so attackers use POST uploads,
data in query strings or headers, chunked transfers, and legitimate
services. In Splunk I filtered POST requests, compared bytes sent per
domain, and isolated unusually large uploads. I then correlated the
finding in a packet capture by filtering large POST requests and
following the HTTP stream to see the exfiltrated content and the
internal host responsible.

### Step 4 - FTP Exfiltration

FTP exposes credentials and file names in cleartext. I filtered the
control and data channels, looked at login commands for suspicious or
guest accounts, searched for upload commands and sensitive file
extensions, and followed TCP streams of large transfers. This
identified which internal host sent the largest payload to an external
address and what kind of file was taken.

### Step 5 - ICMP Exfiltration

Attackers can hide data inside ICMP echo packets because ICMP is
usually allowed and inspected less. Indicators include many echo
requests to one external host, payloads larger than a normal ping,
unusual types or codes, high-entropy payloads, and regular timing. I
filtered echo requests by frame length to find the oversized packets
and inspected their payload.

## Outcome / Classification

Identified and confirmed data exfiltration over all four channels,
located the compromised internal hosts and the receiving external
infrastructure, and recovered the exfiltrated content as evidence.

## Key Takeaways

The same pattern applies to every channel: find where the volume,
size, or frequency of normal traffic is exceeded, then confirm the
content and the source host. A single log source is rarely enough, so
correlating SIEM logs with packet captures made each finding more
certain. It also reinforced the earlier Cyber Kill Chain material,
since exfiltration is part of the final "actions on objectives" stage:
detecting it is a last chance to limit the damage, so it is better to
catch earlier signs such as staging and unusual outbound behaviour.

## Skills Demonstrated

- Detecting DNS, HTTP, FTP, and ICMP exfiltration in packet captures
- Writing Splunk searches to find anomalous query lengths, upload sizes, and per-host activity
- Correlating SIEM log findings with Wireshark packet evidence
- Identifying the compromised internal host, destination, and exfiltrated data
- Using volume, size, frequency, and content as triage indicators
