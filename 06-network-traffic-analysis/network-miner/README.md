# NetworkMiner - TryHackMe SOC Level 1

- **Module:** Network Traffic Analysis
- **Date completed:** 2026-11-15
- **Room link:** https://tryhackme.com/room/networkminer
- **Path:** SOC Level 1

## Overview

This room introduces NetworkMiner, an open-source Network Forensic
Analysis Tool (NFAT) from Netresec. It can work as a passive sniffer,
but its main value is parsing PCAP files to give a quick overview of
hosts, sessions, files, credentials, and anomalies. I used it in a lab
VM to explore several capture files and compared it with Wireshark.

## Objectives

- Understand what NetworkMiner is
- Learn the basics of using it
- Explore its information tabs and extract data efficiently

## Tools / Concepts Covered

- Network forensics and the three data types: live traffic, traffic captures, log files
- NetworkMiner capabilities: PCAP parsing, protocol identification, OS fingerprinting, file extraction, credential grabbing, cleartext keyword parsing
- Operating modes: sniffer mode and packet parsing mode
- Information tabs: Hosts, Sessions, DNS, Credentials, Files, Images, Parameters, Keywords, Messages, Anomalies
- Case panel and file metadata
- Differences between NetworkMiner versions 1.6 and 2.x
- Comparison with Wireshark

## Analysis Process

### Step 1 - Understanding the Tool

NetworkMiner is best used for a fast overview, not as a primary
sniffer or for large-scale manual investigation. The recommended
workflow is to record traffic, get a quick overview in NetworkMiner,
and then go deeper in Wireshark. Its strengths are OS fingerprinting,
easy file extraction, credential grabbing, keyword parsing, and host
categorisation. Its weaknesses are limited filtering, limited packet
decoding, and poor suitability for very large captures.

### Step 2 - Hosts, Sessions, and DNS

The Hosts tab lists every identified host with its IP and MAC address,
operating system guess, open ports, packet counts, and details such
as server banners. Sessions shows individual connections (client and
server, ports, protocol, and start time) with a search bar, and DNS
lists queries and answers. Together these give the context of who
talked to whom before any packet-level work.

### Step 3 - Credentials and Extracted Content

The Credentials tab collects usernames, passwords, and hashes from
supported protocols, such as Kerberos, NTLM, HTTP, FTP, SMTP, and IMAP,
and extracted hashes could be cracked with tools such as Hashcat or
John the Ripper. Files, Images, Parameters, Messages, and Keywords tabs
reconstruct transferred files, pictures, form parameters, emails, and
cleartext keywords, which is much quicker than carving them manually.
Keyword searches require reloading the case files after changing the
keyword list.

### Step 4 - Anomalies and Version Differences

The Anomalies tab flags a few suspicious conditions, such as certain
exploit traces and spoofing attempts, although NetworkMiner is not an
IDS. The lab VM contained both version 1.6 and version 2.x because the
feature sets differ: some features, such as detailed frame and
cleartext views, exist only in the older version, while newer versions
add MAC address correlation and richer parameter extraction. Knowing
this helps me pick the right version for the task.

### Step 5 - Practice with Capture Files

I investigated several capture files by identifying hosts and their
operating systems, reviewing sessions and byte counts between hosts,
extracting credentials, files, images, and messages, and checking the
anomalies tab. Large captures took time to load, so I learned to
reload the file when frames seemed to be missing.

## Outcome / Classification

Completed the questions on host and session analysis, credential and
file extraction, anomalies, version differences, and the two practice
case files.

## Key Takeaways

NetworkMiner is a good first stop for network forensics because it
turns a PCAP into an inventory of hosts, files, and credentials in
minutes, which tells me where to look next. It does not replace
Wireshark: for protocol analysis, filtering, and statistics I still
need packet-level tooling. A sensible workflow is to get the overview
and low-hanging fruit in NetworkMiner and confirm the details in
Wireshark.

## Skills Demonstrated

- Using NetworkMiner to profile hosts, sessions, and DNS activity from a PCAP
- Extracting credentials, files, images, and messages from captured traffic
- Choosing between NetworkMiner and Wireshark for the right stage of an investigation
- Understanding the limits of an overview tool and of features across tool versions
