# Pyramid of Pain - TryHackMe SOC Level 1

- **Module:** Cyber Defence Frameworks
- **Date completed:** 2026-10-28
- **Room link:** [https://tryhackme.com/room/pyramidofpainax](https://tryhackme.com/room/pyramidofpainax)
- **Path:** SOC Level 1

## Overview

This room explains the Pyramid of Pain, a model by David Bianco that
ranks types of indicators by how painful it is for an adversary to
change them when defenders detect and block them. It walks up the
pyramid from trivial indicators to the most costly ones, using
real-world analysis tools and sandbox reports.

## Objectives

- Understand the Pyramid of Pain and its levels
- Learn which indicators are easy or hard for an attacker to change
- Practice analysing indicators with public tools and sandbox reports
- Connect detection types to the effort they force on the adversary

## Tools / Concepts Covered

- Hash values (MD5, SHA-1, SHA-256) and hash lookup tools (VirusTotal, MetaDefender)
- IP addresses and Fast Flux DNS
- Domain names, Punycode attacks, and URL shorteners
- Host artifacts and network artifacts (such as user-agent strings)
- Attacker tools, antivirus signatures, detection rules, YARA, and fuzzy hashing
- TTPs and the MITRE ATT&CK matrix
- Any.Run sandbox reports, TShark, Snort / IDS concepts

## Analysis Process

### Step 1 - Hash Values (Trivial)

A hash uniquely identifies a file, which makes it a precise indicator,
but changing even one bit of a file produces a completely different
hash. I saw this with a simple demonstration of appending text to a
file. Hashes are useful for identifying a known sample through lookup
tools, but weak for blocking an adversary who can recompile at will.

### Step 2 - IP Addresses (Easy)

Blocking IPs is a common defence, but an attacker can switch to a new
address quickly. Techniques such as Fast Flux, where a domain rotates
across many IP addresses, make this even harder to rely on. I reviewed
a sandbox report to see which addresses a malicious process contacted.

### Step 3 - Domain Names (Simple)

Domains cost an attacker more effort than IPs because they must
register them and update DNS records, but providers often make it
easy. I covered Punycode homograph attacks, where look-alike Unicode
characters imitate a trusted domain, and URL shorteners that hide the
real destination (which can be previewed safely before visiting). In
the sandbox, the networking tabs (HTTP, connections, DNS requests)
show what the malware contacts.

### Step 4 - Host and Network Artifacts (Annoying)

Host artifacts are traces left on a system, such as registry changes,
suspicious process execution, and dropped files. Network artifacts
include user-agent strings, C2 information, and URI patterns. I
reviewed examples of both and saw how a distinctive user-agent
extracted from packet captures with a tool like TShark can identify a
malware family. Detecting these forces the attacker to change their
tooling or methods.

### Step 5 - Tools (Challenging)

Detecting the attacker's tools means detecting custom binaries,
malicious macro documents, backdoors, and password crackers. Defences
here include antivirus signatures, detection rules, and YARA rules, and
sources such as malware sample repositories and detection rule
marketplaces. Fuzzy hashing helps find similarity between files that
differ slightly, which counters small modifications.

### Step 6 - TTPs (Tough)

TTPs describe how an adversary operates, covering the whole MITRE
ATT&CK matrix. Detecting behaviours such as credential reuse for
lateral movement forces the attacker to change their entire approach
or give up. I explored the ATT&CK matrix and attributed tools to a
known threat group.

### Step 7 - Practice

I finished with an interactive exercise placing indicators into the
correct tier of the pyramid.

## Outcome / Classification

Completed all indicator-analysis questions and correctly classified
the indicators in the final pyramid exercise.

## Key Takeaways

The most useful idea is that detection effort should aim as high on
the pyramid as practical. Blocking hashes and IPs is quick and easy
but short-lived, while detections based on behaviour and TTPs are
harder to build yet cost the attacker the most to evade. As an analyst
I should always ask where an indicator sits on the pyramid, since that
tells me how long a block or detection rule is likely to stay
effective.

## Skills Demonstrated

- Ranking indicators of compromise by durability and attacker cost
- Using hash, IP, and domain lookups and sandbox reports to analyse samples
- Recognising Fast Flux, Punycode, and URL-shortener evasion techniques
- Understanding how signatures, YARA rules, and behaviour-based detection fit together
- Mapping adversary behaviour to MITRE ATT&CK
