# MITM Detection - TryHackMe SOC Level 1

- **Module:** Network Security Monitoring
- **Date completed:** 2026-11-16
- **Room link:** https://tryhackme.com/room/mitmdetection
- **Path:** SOC Level 1

## Overview

This room covers Man-in-the-Middle (MITM) attacks from a defender's
point of view. I acted as a SOC analyst investigating an incident at a
fictional company, where an attacker had chained three MITM
techniques to intercept traffic and capture credentials: ARP
spoofing, DNS spoofing, and SSL stripping. I used a packet capture in
Wireshark (and the same logs were available in Splunk) to find the
evidence for each stage.

## Objectives

- Understand common MITM attack vectors and techniques
- Identify indicators of compromise related to MITM attacks
- Use network monitoring tools to detect suspicious traffic patterns
- Practise investigation and response for MITM scenarios

## Tools / Concepts Covered

- MITM attack types: packet sniffing, session hijacking, SSL stripping, DNS spoofing, IP spoofing, rogue Wi-Fi access points
- MITM within the Cyber Kill Chain (exploitation and installation phases)
- ARP protocol weaknesses and ARP spoofing indicators
- DNS spoofing indicators (conflicting responses, unexpected sources, short TTLs)
- SSL stripping indicators (HTTPS downgraded to HTTP, redirects, certificate errors)
- Wireshark display filters for ARP, DNS, TLS, and HTTP
- Attack timeline reconstruction

## Analysis Process

### Step 1 - MITM and the Kill Chain

A MITM attack involves interception and then manipulation or
decryption of the communication. Mapping it to the Cyber Kill Chain
helped me place it: it works as an exploitation technique because it
abuses the trust built into protocols such as ARP and DNS, and as an
installation vector because an attacker in the middle can inject
malicious payloads into unencrypted traffic. Detecting MITM means the
adversary is already in the middle stages of an intrusion, which gives
the SOC a chance to interrupt the attack.

### Step 2 - ARP Spoofing

ARP has no authentication, so any host can claim an IP address. The
attacker sent forged replies tying the gateway's IP to their own MAC
address, so traffic flowed through them. I looked for duplicate
IP-to-MAC mappings, unsolicited and gratuitous ARP replies, unusual
volumes of ARP traffic, and multiple MAC addresses answering for the
gateway. Filtering ARP replies for the gateway's IP and then by the
suspicious MAC address confirmed the poisoning, and Wireshark's
duplicate-address detection corroborated it.

### Step 3 - DNS Spoofing

With the attacker positioned in the middle, forged DNS replies sent
the victim to the attacker's server instead of the real one. The
strongest indicator is more than one response to the same query, with
one coming from an unexpected source rather than the legitimate
resolver. I compared normal resolver responses with replies from other
sources for the target domain to isolate the forged answers and see
which address the victim was redirected to.

### Step 4 - SSL Stripping

The last stage downgraded the victim's secure connection to plain
HTTP. The target site normally uses TLS, as the handshakes showed, but
after the redirect the victim's traffic to the attacker was
unencrypted, and the login POST request exposed the credentials in
cleartext. Indicators include a switch from HTTPS to HTTP for the same
domain, redirects pointing to HTTP, and certificate errors.

### Step 5 - Attack Timeline

I summarised the incident as a chain: ARP poisoning to get in the
middle, forged DNS responses to redirect the victim, then SSL
stripping to capture the credentials.

## Outcome / Classification

Confirmed a multi-stage MITM attack and identified the evidence for
each stage: ARP spoofing, DNS spoofing, and SSL stripping with a
credential capture. Related logs were also available in Splunk for
correlation.

## Key Takeaways

Each technique leaves a clear signature if I know the normal behaviour
of the protocol: one IP with two MAC addresses, one DNS query with two
answers, and a secure site suddenly speaking plain HTTP. The attack
also showed why single indicators are not enough. Following the chain
from ARP to DNS to HTTP gave the full story and showed how the stolen
credentials were obtained. For response, an incident like this should
lead to resetting the exposed credentials, isolating the attacker's
host, and hardening against repeat attacks, for example with HSTS,
DNS protections, and ARP monitoring.

## Skills Demonstrated

- Detecting ARP spoofing through duplicate mappings and gratuitous replies
- Identifying forged DNS responses and rogue resolvers
- Recognising SSL stripping and cleartext credential exposure
- Mapping MITM techniques to the Cyber Kill Chain
- Reconstructing a multi-stage attack timeline from packet captures
