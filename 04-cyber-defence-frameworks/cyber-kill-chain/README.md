# Cyber Kill Chain - TryHackMe SOC Level 1

- **Module:** Cyber Defence Frameworks
- **Date completed:** 2026-10-28
- **Room link:** https://tryhackme.com/room/cyberkillchainzmt/
- **Path:** SOC Level 1

## Overview

This room covers the Lockheed Martin Cyber Kill Chain, a framework that
breaks an intrusion into seven phases. Understanding the phases helps a
SOC analyst recognise what stage an attack has reached, what the
adversary is trying to achieve, and where the chain can be broken. The
room finishes with a real-world breach analysis and a discussion of the
framework's limitations.

## Objectives

- Learn each phase of the Cyber Kill Chain
- Recognise attacker techniques and indicators at each phase
- Apply the framework to a real-world breach scenario
- Understand the advantages and disadvantages of the traditional model

## Tools / Concepts Covered

- The seven phases: Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command and Control, Actions on Objectives
- OSINT, passive vs. active reconnaissance, email harvesting
- Malware, exploits, and payloads; malicious macros
- Delivery methods: phishing, USB drops, watering hole attacks
- Zero-day and known CVE exploitation
- Persistence: web shells, backdoors, services, run keys, timestomping
- C2 channels: HTTP/HTTPS beaconing and DNS tunnelling
- MITRE ATT&CK and the Unified Kill Chain as complements

## Analysis Process

### Step 1 - Reconnaissance and Weaponization

Reconnaissance is the research phase, either passive (no direct
contact, such as WHOIS lookups and social media research) or active
(port scanning, banner grabbing). OSINT and email harvesting give the
attacker targets for phishing. In weaponization the attacker turns
that information into attack tools by building or buying malware and
combining it with a delivery vehicle, such as a macro-enabled document
or a backdoor with C2 infrastructure.

### Step 2 - Delivery and Exploitation

Delivery is how the payload reaches the victim: phishing emails,
USB drops, or watering hole attacks that compromise a site a target
group visits. Exploitation is when the code actually runs, through
macros, zero-day exploits, or known unpatched CVEs. Signs to look for
include unexpected process spawns, registry changes, new services, and
suspicious command-line arguments.

### Step 3 - Installation and Command and Control

After execution, attackers install persistence so they can regain
access if disconnected or patched out: web shells, backdoors, created
or modified services, and registry run keys or startup folders. They
may also alter file timestamps to hide their changes. Once
established, the compromised host beacons to a C2 server, often over
HTTP or HTTPS to blend in, or over DNS requests to a domain the
attacker controls.

### Step 4 - Actions on Objectives

With hands-on access, the attacker can harvest credentials, escalate
privileges, perform internal reconnaissance, move laterally, exfiltrate
data, and delete backups or corrupt data to cause maximum damage.

### Step 5 - Real-World Practice and Limitations

I applied the framework to a well-known retail breach by placing the
attack techniques into the correct kill chain stages in an interactive
exercise. The room also explained why the model has limits: it was
designed around perimeter defence and malware, has not been updated
since 2011, and does not handle insider threats well, so it should be
combined with MITRE ATT&CK and the Unified Kill Chain.

## Outcome / Classification

Completed the questions on each phase and placed the techniques of the
breach scenario into the correct kill chain stages.

## Key Takeaways

The kill chain is useful because it turns a single alert into a
position on a timeline. If I see persistence or C2 activity, I know
earlier phases have already happened, so I should look backwards for
the initial access and forwards for lateral movement and data theft.
The attacker must complete every phase, so detecting or blocking any
single one can break the chain. It also helps to remember its
weaknesses, which is why ATT&CK is used alongside it.

## Skills Demonstrated

- Mapping attacker activity to the seven Cyber Kill Chain phases
- Recognising indicators of exploitation, persistence, and C2 communication
- Explaining common delivery methods and persistence techniques
- Understanding where the Cyber Kill Chain falls short and how ATT&CK complements it
