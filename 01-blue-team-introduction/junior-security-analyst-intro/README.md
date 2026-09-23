# Junior Security Analyst Intro — TryHackMe SOC Level 1

**Module:** Blue Team Introduction
**Date completed:** 2026-09-22
**Room link:** https://tryhackme.com/room/junsecurityanalystintro
**Path:** SOC Level 1

## Overview

This room simulates a day in the life of a Security Analyst working in a
SOC — from the morning shift handoff and reviewing overnight alerts, to
triaging suspicious events, and escalating a confirmed incident to a
Tier 2 analyst. The focus isn't deep technical analysis, but
understanding the **workflow, responsibilities, and mindset** of an L1
analyst.

## Objectives

- Understand the daily responsibilities of a SOC L1 analyst
- Learn the shift handoff process and why it matters
- Practice alert triage and prioritization
- Understand when and how to escalate an incident

## Tools / Concepts Covered

- SIEM dashboard (simulated environment)
- Alert triage workflow: Detect → Investigate → Escalate/Close
- Alert severity levels (Critical / High / Medium / Low)
- SOC shift handoff documentation
- Incident escalation reporting

## Analysis Process

### Step 1 — Morning Check (Shift Handoff)

The first task of the shift is reviewing notes left by the previous
analyst and any open alerts carried over from the night shift. This
step is critical — missing handoff information can lead to duplicated
work or, worse, missing an active incident that is already in progress.

### Step 2 — Alert Triage

The room presents a list of alerts sorted by severity
(Critical/High/Medium/Low). I worked through the following logic:

| Severity | Action |
|---|---|
| Critical / High | Investigate immediately, potential escalation |
| Medium | Investigate in queue order, check context (IP reputation, known false positives) |
| Low | Document and close if no indicator of compromise is found |

**Key takeaway:** an L1 analyst does not resolve every issue alone —
the job is to identify, add context, and hand off appropriately when
needed.

### Step 3 — Escalation

When an alert shows signs of a genuine compromise (e.g. repeated
failed login attempts followed by a successful login from an unusual
location), the room walks through writing an escalation report:
what happened, why it's suspicious, what evidence was gathered, and
the recommended urgency level.

## Outcome / Classification

Alert classified as **True Positive — Suspicious Login Activity**,
escalated to the Tier 2 team with a complete incident timeline.

## Key Takeaways

This room made it clear that L1 SOC work is much more about **process
and communication** than deep technical analysis — clear documentation
and accurate prioritization matter just as much as technical skill.
Coming from a DevOps background, this felt familiar — it closely
mirrors the incident response process I used for production issues,
just with a different focus (security rather than uptime).

## Skills Demonstrated

- Alert triage and prioritization
- SOC shift handoff procedure
- Incident escalation documentation
- Analytical decision-making under a structured workflow
