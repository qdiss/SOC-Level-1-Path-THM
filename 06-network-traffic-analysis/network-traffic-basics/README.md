# Network Traffic Basics - TryHackMe SOC Level 1

- **Module:** Network Traffic Analysis
- **Date completed:** 2026-11-14
- **Room link:** https://tryhackme.com/room/networktrafficbasics  
- **Path:** SOC Level 1

## Overview

This is the first room of the Network Traffic Analysis module. It
explains what network traffic analysis (NTA) is, why a SOC needs it,
what can be observed at each layer of the TCP/IP model, which sources
and flows exist in a corporate network, and how traffic is captured
and measured. NTA is more than a single tool: it combines logs, deep
packet inspection, and network flow statistics around a specific goal.

## Objectives

- Understand what network traffic analysis is
- Know what can be observed in network traffic
- Know how to observe network traffic
- Know the typical traffic sources and flows in a network

## Tools / Concepts Covered

- The TCP/IP model and what each layer's headers reveal
- Logs vs. full packet capture vs. network statistics
- Traffic sources: intermediary devices and endpoint devices
- Traffic flows: North-South and East-West
- Example flows: HTTPS with TLS inspection, external DNS, SMB with Kerberos
- Network TAPs and port mirroring (SPAN)
- Packet analysis tools (Wireshark, tcpdump, Snort, Suricata, Zeek)
- NetFlow and IPFIX

## Analysis Process

### Step 1 - Why Network Traffic Analysis

Logs from firewalls and other devices record only fragments of what
happened, such as DNS query names or source and destination
addresses, but not the content. A scenario with unusual DNS queries to
one domain with random subdomains showed the gap: only inspecting the
actual DNS replies reveals whether they carry hidden commands. NTA
helps a SOC detect malicious activity, reconstruct attacks during
incident response, and verify alerts, as well as monitor general
network performance.

### Step 2 - What Can Be Observed per Layer

At the application layer, proxies and firewalls usually log headers
(for example an HTTP request and response code) but not the payload,
such as the contents of a downloaded file. At the transport layer,
logs show ports and flags, while the full headers, including sequence
numbers, can expose attacks such as session hijacking. At the internet
layer, normally logged fields like IP addresses and TTL are
insufficient for detecting fragmentation attacks, which need
fragment-level details. At the link layer, attacks such as ARP
poisoning only become visible in packet captures where conflicting
MAC and IP mappings can be compared.

### Step 3 - Sources and Flows

Traffic comes from intermediary devices (firewalls, switches, routers,
proxies) and endpoints (servers, hosts, IoT, printers, cloud
resources), with endpoints producing most of the volume. Flows are
either North-South (entering or leaving the LAN through the firewall)
or East-West (internal). East-West traffic is monitored less often,
but it matters because attackers use internal services for lateral
movement. I walked through three example flows to understand where
visibility exists: HTTPS via an inspecting proxy, DNS resolution
through an internal server, and SMB access after Kerberos
authentication.

### Step 4 - How to Observe Traffic

There are three main sources of information: logs, full packet
capture, and network statistics. Full packet capture can be done with
a physical network TAP, which copies traffic without added delay, or
with port mirroring (SPAN) on a switch, which is software based but
can affect performance under heavy load. Placement, storage needs for
long captures, and the TAP versus mirror trade-off all need planning.
For statistics, NetFlow and its vendor-neutral successor IPFIX provide
flow metadata that can help detect C2 traffic, data exfiltration, and
lateral movement without storing every packet.

### Step 5 - Practical Exercise

In an interactive exercise I positioned the capture point correctly
and inspected captured traffic to find hidden data in HTTP and DNS
traffic.

## Outcome / Classification

Completed all concept questions and both practical capture scenarios.

## Key Takeaways

Logs tell me that something happened, while packet captures and flow
data let me prove what happened. The most useful lesson was matching
each type of attack to the layer and data source that can reveal it,
for example payloads for application-layer threats, sequence numbers
for session hijacking, and MAC-to-IP mappings for ARP poisoning. It
also showed why East-West visibility should not be neglected. This
prepares me for the next rooms, where I will practise packet analysis
hands-on with Wireshark.

## Skills Demonstrated

- Explaining what each TCP/IP layer reveals and which attacks it helps detect
- Distinguishing North-South from East-West traffic and the associated monitoring needs
- Choosing between logs, full packet capture, and flow data for an investigation
- Understanding TAP and port mirroring placement and trade-offs
- Recognising how DNS and HTTP traffic can hide malicious content
