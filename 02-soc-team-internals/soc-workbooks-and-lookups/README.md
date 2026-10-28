# SOC Workbooks and Lookups - TryHackMe SOC Level 1

- **Module:** SOC Team Internals
- **Date completed:** 2026-10-28
- **Room link:** [https://tryhackme.com/room/socworkbooksandlookups](https://tryhackme.com/room/socworkbookslookups)
- **Path:** SOC Level 1

## Overview

This room covers the context sources an L1 analyst uses during alert
triage (identity inventory, asset inventory, and network diagrams) and
SOC workbooks, the structured guides that keep investigations
consistent. It ends with an interactive exercise building workbooks
step by step.

## Objectives

- Understand SOC investigation workbooks
- Learn where to find and how to use identity and asset inventories
- Understand the value of corporate network diagrams
- Practice building investigation workflows in an interactive interface

## Tools / Concepts Covered

- Identity inventory (Active Directory / Entra ID, SSO providers, HR systems, custom sheets)
- Asset inventory (Active Directory, SIEM/EDR, MDM solutions, custom sheets)
- Network diagrams (subnets, exposed services, firewall boundaries)
- SOC workbooks / playbooks / runbooks
- Enrichment → Investigation → Escalation structure

## Analysis Process

### Step 1 - Getting Context on Users and Hosts

For an alert like a user logging into a file server and sharing a
sensitive file, the verdict depends on context: who the users are,
their roles and working hours, what the server is used for, and who
should have access. Identity inventory answers the first set of
questions and asset inventory answers the second. I used both to
judge whether a file-sharing scenario looked expected or suspicious.

### Step 2 - Using Network Diagrams

For network-based alerts, raw IPs mean little without context. I used
a network diagram to identify which service was exposed on a given
port and which subnet an internal address belonged to. With that, I
reconstructed an attack path across a chain of firewall alerts: a VPN
brute force from an external IP, followed by internal scanning of
other subnets from the assigned VPN address, and reached a verdict
for the scenario.

### Step 3 - Understanding SOC Workbooks

Workbooks are structured guides that define the steps to investigate
and remediate a specific alert type, written mostly by senior
analysts to support L1. The example workbook for unusual login
locations was split into three logical groups: enrichment (threat
intelligence and identity lookups), investigation (SIEM logs and a
verdict), and escalation (L2 or contacting the user). Following the
order prevents verdicts made without enough evidence.

### Step 4 - Building Workbooks

In the interactive exercise I completed three workbooks by placing the
missing steps in the correct positions, which reinforced how to break
an investigation into modular blocks.

## Outcome / Classification

Completed the context-lookup questions and the three workbook-building
exercises.

## Key Takeaways

A correct verdict often depends less on the log itself than on context
around it: who the user is, what the host does, and where an IP sits
in the network. Workbooks make that context gathering repeatable and
protect junior analysts from skipping steps. Coming from a systems
background, I found network diagrams and asset inventories familiar,
since they mirror documentation I used when working with servers.

## Skills Demonstrated

- Using identity and asset inventories to judge whether activity is expected
- Reading network diagrams to reconstruct an attack path
- Following and structuring investigation workbooks (enrichment, investigation, escalation)
- Distinguishing True Positive from False Positive using contextual evidence
