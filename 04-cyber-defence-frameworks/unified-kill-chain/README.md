# Unified Kill Chain - TryHackMe SOC Level 1

- **Module:** Cyber Defence Frameworks
- **Date completed:** 2026-10-28
- **Room link:** https://tryhackme.com/room/unifiedkillchain
- **Path:** SOC Level 1

## Overview

This room introduces the Unified Kill Chain (UKC), a framework created
to complement models such as Lockheed Martin's Cyber Kill Chain and
MITRE ATT&CK. It describes a full attack from initial reconnaissance
to final objectives, and models the fact that attackers often loop
back through earlier phases. I used it to classify attacker activity
and to reason about where defences could interrupt an attack.

## Objectives

- Understand why frameworks like the UKC help build a good security posture
- Use the UKC to understand attacker motivation, methods, and tactics
- Learn the phases of the UKC
- Understand how the UKC complements other frameworks such as MITRE ATT&CK

## Tools / Concepts Covered

- Kill chains as a model of attacker methodology
- Threat modelling: identifying assets, weaknesses, mitigation plans, and preventive policies
- UKC compared with other frameworks (more detailed, covers the whole attack, allows repeated phases)
- Three groups of phases: In, Through, Out
- Mapping phases to MITRE ATT&CK tactics
- The CIA triad (confidentiality, integrity, availability)

## Analysis Process

### Step 1 - Why Kill Chains and Threat Modelling

A kill chain describes the path an attacker takes against a target.
Understanding it lets defenders put controls in place ahead of time or
disrupt an attack in progress. This connects to threat modelling:
identify which systems matter, assess their weaknesses, plan how to
secure them, and set policies (for example secure development
practices or phishing awareness training) to prevent recurrence.

### Step 2 - How the UKC Differs

The UKC is more modern and much more detailed than traditional kill
chains, covers the whole attack including post-exploitation and
motivation, and reflects reality better because phases can repeat. For
example, after compromising one machine an attacker returns to
reconnaissance to find the next target.

### Step 3 - "In": Gaining a Foothold

This group covers getting access and keeping it: reconnaissance,
weaponization (setting up infrastructure such as C2), social
engineering, exploitation, persistence, defence evasion, and command
and control, followed by pivoting. Defence evasion is especially
valuable to study because it shows how an attacker bypasses firewalls,
antivirus, and intrusion detection, which tells defenders where to
improve.

### Step 4 - "Through": Expanding Access

After a foothold, the attacker uses a pivot system to discover
accounts, permissions, and network resources, escalate privileges,
execute further code, steal credentials, and move laterally. I
practised classifying alerts, such as repeated failed administrator
logins and credential-dumping tools, into the right phase.

### Step 5 - "Out": Achieving Objectives

The final group covers collection, exfiltration, and impact, ending
with the attacker's strategic objectives. These map onto the CIA
triad: collection and exfiltration affect confidentiality, while
destruction or encryption affects integrity and availability. I
classified scenarios such as unusual outbound traffic spikes and the
public release of personal data.

### Step 6 - Practice

I finished with an interactive exercise matching attacker actions to
the correct UKC phase.

## Outcome / Classification

Completed the phase-classification questions and the matching exercise.

## Key Takeaways

The UKC is most useful when I treat an alert as a clue about the
attacker's current position in a longer attack. A credential-dumping
tool points to the "Through" group, so I should look for lateral
movement next, while a large outbound transfer points toward the "Out"
group and possible data theft. Comparing the UKC with the Cyber Kill
Chain and ATT&CK showed me that no single framework is enough, and
that combining them gives a fuller picture.

## Skills Demonstrated

- Classifying SOC alerts into Unified Kill Chain phases
- Relating UKC phases to MITRE ATT&CK tactics
- Explaining how kill chain frameworks support threat modelling
- Linking attacker actions to impact on the CIA triad
