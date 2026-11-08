# Security Orchestration, Automation, and Response (SOAR) - TryHackMe SOC Level 1

- **Module:** Core SOC Solutions
- **Date completed:** 2026-10-28
- **Room link:** https://tryhackme.com/room/soar
- **Path:** SOC Level 1

## Overview

This room explains how a traditional SOC works, the challenges it
faces, and how a SOAR platform addresses them through orchestration,
automation, and response. It also covers playbooks with two real
examples and ends with an exercise on building a threat intelligence
workflow.

## Objectives

- Understand the traditional SOC and its challenges
- Learn how SOAR overcomes those challenges
- Learn what SOAR playbooks are and how they are structured
- Walk through a threat intelligence workflow in practice

## Tools / Concepts Covered

- Traditional SOC capabilities: monitoring and detection, recovery and remediation, threat intelligence, communication
- SOC challenges: alert fatigue, disconnected tools, manual processes, talent shortage
- SOAR capabilities: orchestration, automation, response
- Playbooks (phishing and CVE patching examples)
- Manual vs. automated steps in a workflow

## Analysis Process

### Step 1 - Challenges of a Traditional SOC

A SOC relies on many tools (SIEM, EDR, firewalls, IAM, threat
intelligence) and communicates with IT and management. The room
highlighted four recurring problems: alert fatigue from too many
alerts and false positives, disconnected tools that force analysts to
switch between consoles, undocumented manual processes based on
tribal knowledge, and a shortage of skilled analysts.

### Step 2 - How SOAR Helps

SOAR unifies the security tools in one interface through three
capabilities. **Orchestration** connects tools from different vendors
and defines investigation workflows. **Automation** lets the platform
follow those workflows without manual clicks. **Response** allows
actions such as blocking an IP, disabling a user, or opening a ticket
from one place. A VPN brute force alert was used as the example: the
workflow checks the user's login history, verifies the IP reputation,
checks for successful logins, and escalates to containment, with each
step's result deciding the next one.

### Step 3 - Playbooks

A playbook is a predefined set of steps and decision branches for a
category of recurring alerts, similar to a flow diagram a senior
analyst would give a junior. I reviewed two examples: a phishing
playbook (create a ticket, check for URLs and attachments, analyse
them with threat intelligence, then remediate or notify) and a CVE
patching playbook (analyse the CVE, assess risk, create a patching
ticket, test the patch before production). In both, most steps are
automated but analysts remain at key decision points.

### Step 4 - Practical Workflow

In the hands-on exercise I configured a threat intelligence
integration workflow by deciding which steps should be automated and
which should stay manual, then ran it to validate the configuration.

## Outcome / Classification

Completed the threat intelligence workflow by correctly assigning
manual and automated steps.

## Key Takeaways

SOAR does not replace analysts. It removes repetitive work so analysts
can spend time on judgment calls that depend on business context, and
analysts are the ones who design the playbooks in the first place. The
choice of which steps to automate matters: low-risk, repeatable checks
suit automation, while decisions with business impact, such as
disabling a user, may need a human in the loop. This connects to
earlier modules: workbooks written for L1 analysts are essentially
playbooks that SOAR can execute.

## Skills Demonstrated

- Explaining orchestration, automation, and response in a SOC context
- Reading and reasoning about playbook flow diagrams
- Deciding which workflow steps to automate and which to keep manual
- Identifying SOC pain points and how tooling addresses them
