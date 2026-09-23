# Junior Security Analyst Intro - TryHackMe SOC Level 1

- **Module:** Blue Team Introduction
- **Date completed:** 2026-09-23
- **Room link:** https://tryhackme.com/room/jrsecanalystintrouxo
- **Path:** SOC Level 1

## Overview

This room introduces the structure of a Security Operations Center
(SOC) and the different roles within it, then walks through a
simulated day in the life of a Security Analyst - from daily routines
to hands-on alert investigation and escalation.

## Objectives

- Understand the different roles that make up a SOC team
- Learn the daily responsibilities of a Security Analyst
- Practice investigating a security alert end-to-end
- Practice escalating and containing a confirmed threat

## Tools / Concepts Covered

- SOC team structure (Analyst, Engineer, Manager, Incident Responder)
- IP reputation/investigation tooling ("IP Hunter")
- Alert triage and escalation workflow
- Basic containment action (blocking a malicious IP)

## Analysis Process

### Step 1 - Understanding the SOC Team Structure

Before touching any alert, the room breaks down the different roles in
a SOC: the Security Analyst (first line of defense, monitors and
investigates), the SOC Engineer (maintains the tools that generate
alerts), the SOC Manager (runs the team day-to-day), and the Incident
Responder (only engaged for major incidents). Understanding who does
what clarified where an L1 analyst's responsibility starts and ends -
and when something should be handed off rather than handled solo.

### Step 2 - A Day in the Life (Sam's Journal)

The room includes an interactive walkthrough of a typical analyst's
day - reviewing security news, checking alert queues, attending team
discussions, and coordinating with other teams when an issue crosses
departments. This reinforced that the job isn't just reactive
alert-clicking; staying aware of current threats and campaigns in the
wild is part of the daily routine, since it directly informs what to
watch for internally.

### Step 3 - Hands-On Alert Investigation

The practical scenario required identifying the relevant alert in a
queue, extracting the suspicious IP address, and investigating it
using an IP reputation lookup tool. Based on the findings, the next
steps were to escalate the alert to the appropriate person and take a
containment action - blocking the IP to stop further attempts while
the investigation continued.

## Outcome / Classification

**True Positive** - the investigated IP was confirmed malicious.
Escalated per the room's workflow and blocked as a containment
measure to prevent further access attempts.

## Key Takeaways

This room made the SOC hierarchy concrete for me - I now have a
clearer picture of where an L1 analyst's role starts and ends, and why
escalation paths exist rather than expecting one person to resolve
everything. The hands-on investigation also reinforced a repeatable
pattern I'll keep using: identify the indicator (IP), enrich it with
reputation data, decide escalate vs. close, then contain if confirmed
malicious.

## Skills Demonstrated

- Understanding of SOC team roles and escalation paths
- IP reputation investigation
- Alert triage and escalation decision-making
- Basic incident containment (IP blocking)
