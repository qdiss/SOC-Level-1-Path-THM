# Systems as Attack Vectors - TryHackMe SOC Level 1

- **Module:** Blue Team Introduction
- **Date completed:** 2026-09-23
- **Room link:** https://tryhackme.com/room/systemsattackvectors
- **Path:** SOC Level 1

## Overview

This room complements the human-focused attack room by covering the
other side of the SOC's job: systems as attack vectors. It looks at
why systems are valuable targets, the main ways they get breached, and
how a SOC analyst mitigates and detects these attacks - practiced
through two dashboard-based scenarios.

## Objectives

- Understand the role of systems in the modern digital world
- Explore real-world attacks that target systems directly
- Practice the concepts in two realistic scenarios

## Tools / Concepts Covered

- System types and their attack value (endpoint vs. server vs. cloud platform)
- Human-led attacks (weak/reused passwords, malicious USB, pirated software)
- Software vulnerabilities and CVEs, including zero-days
- Supply chain attacks (e.g. SolarWinds, 3CX-style compromises)
- Misconfigurations vs. vulnerabilities
- Mitigation measures: patch management, IT training, network restriction, antivirus/EDR
- SOC dashboard simulation ("Systems at Risk", "Remediation Plan")

## Analysis Process

### Step 1 - Why Systems Are High-Value Targets

The room reframes the earlier "human as weakest link" idea: even with
a well-trained user, an insecure system can be attacked directly,
without any user interaction at all. It also highlighted a key scaling
factor - breaching one mailbox affects one person, but breaching the
mail server behind it can expose every mailbox on that system. This
made the case for why systems deserve equal attention to humans in a
SOC's threat model.

### Step 2 - How Systems Get Breached

Three main attack paths were covered:

- **Human-led attacks** - weak/reused passwords, malicious USB drops,
  malware from pirated software - where a person's action is still
  the entry point, but the target is the system itself.
- **Vulnerabilities** - software flaws tracked as CVEs, including
  zero-days (flaws attackers find before defenders do), which put the
  SOC in a race between patch availability and active exploitation.
- **Supply chain attacks** - compromising a trusted app or library
  upstream so that a routine update delivers malware to everyone using
  it, which is especially hard to defend against since you don't
  control every dependency running in your environment.

### Step 3 - Vulnerabilities vs. Misconfigurations

An important distinction the room draws out: a vulnerability is a flaw
in the software itself, fixed by a vendor patch, while a
misconfiguration is a mistake in how a system was set up (e.g. default
or weak credentials, overly permissive access) and requires a setup
fix rather than a patch. Response approaches differ accordingly -
patch management and exploitation monitoring for vulnerabilities,
versus configuration audits, vulnerability scans, and penetration
testing for misconfigurations.

### Step 4 - Practical Scenarios

The hands-on lab simulated a SOC dashboard with two linked tasks:
investigating "Systems at Risk" to identify what needed attention, and
then selecting the correct actions on a "Remediation Plan" tab to
address them - combining detection (identifying the risk) with
mitigation (choosing the right fix, e.g. patch vs. reconfigure).

## Outcome / Classification

Completed both practical scenarios ("Systems at Risk" and "Remediation
Plan") successfully, correctly distinguishing between patching and
reconfiguration as remediation actions.

## Key Takeaways

This room sharpened the distinction between a vulnerability and a
misconfiguration for me - which matters practically, since the right
response (patch vs. reconfigure) is different for each, and picking
the wrong one wastes time during an actual incident. It also reinforced
that attackers don't distinguish between "human" and "system" attack
paths - as an analyst I need to watch both with equal attention rather
than treating system security as someone else's job.

## Skills Demonstrated

- Distinguishing vulnerabilities from misconfigurations and their respective fixes
- Awareness of supply chain attack risk and its detection challenges
- Applying patch management and remediation-planning logic in a simulated SOC scenario
- Connecting CVE/zero-day concepts to real-world SOC response timing
