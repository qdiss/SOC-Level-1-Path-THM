# SOC L1 Alert Triage — TryHackMe SOC Level 1

- **Module:** SOC Team Internals
- **Date completed:** 2026-10-28
- **Room link:** [https://tryhackme.com/room/soctriage](https://tryhackme.com/room/socl1alerttriage/)
- **Path:** SOC Level 1

## Overview

This room explains what a SOC alert is, how it is generated from raw
events, what information an alert contains, and how an L1 analyst
should prioritise and triage alerts. It ends with hands-on practice in
a simulated SOC dashboard, where I took alerts through the full triage
lifecycle.

## Objectives

- Understand the concept of a SOC alert and how it is formed
- Learn alert fields, statuses, and verdicts
- Learn how to prioritise alerts in a queue
- Practice the full triage flow on real-style alerts in a SIEM dashboard

## Tools / Concepts Covered

- Event → log → alert pipeline (SIEM / EDR / NDR / SOAR / ITSM platforms)
- Alert properties: time, name, severity, status, verdict, assignee, description, fields
- Alert prioritisation logic (filter, sort by severity, sort by time)
- Triage flow: initial actions, investigation, final actions
- True Positive vs. False Positive classification
- TryHackMe SIEM / SOC dashboard simulation

## Analysis Process

### Step 1 — Understanding How Alerts Are Born

An event (login, process start, file download) is logged by a system,
shipped to a SIEM or EDR, and turned into an alert when it matches a
detection rule. This is what lets analysts review dozens of alerts a
day instead of millions of raw logs. I also reviewed the platforms
that manage alerts (SIEM, SOAR, ITSM) and how each SOC role touches
the process: L1 triages and escalates, L2 does deeper analysis,
engineers keep alerts informative, and managers track triage speed and
quality.

### Step 2 — Reading an Alert

I went through the main alert properties: alert time (usually a few
minutes after the real event), name, severity, status, verdict,
assignee, description, and alert fields such as hostname or command
line. In the dashboard I reviewed an existing closed alert to see how
verdict and context fit together.

### Step 3 — Prioritising the Queue

The standard logic I applied was: first filter out alerts already
being handled by someone else, then sort by severity (critical first),
then by time (oldest first). The reasoning is that higher-severity
rules are more likely to indicate real, high-impact threats, and an
older alert may mean the attacker is already further into the
intrusion than one that just started.

### Step 4 — Triage Flow

I followed the reference triage flow:

- **Initial actions:** assign the alert to myself, move it to In
  Progress, and read its name, description and key indicators
- **Investigation:** identify who or what is affected, understand the
  action described, review surrounding events before and after, and
  verify findings with threat intelligence
- **Final actions:** decide True Positive or False Positive, write a
  comment explaining my steps and reasoning, and move the alert to
  Closed

I repeated this on three alerts in priority order.

## Outcome / Classification

Completed triage of three alerts in the correct priority order, each
with the correct verdict and a documented reasoning comment.

## Key Takeaways

The biggest lesson is that triage is a repeatable process, not
guesswork: filter, prioritise, take ownership, investigate with
context, decide, and document. Taking ownership of an alert before
investigating avoids duplicated work in a team, and the written
comment matters as much as the verdict, since the next analyst may
depend on it. The room also made clear that closing an alert as True
Positive is only the start; escalation and reporting come next.

## Skills Demonstrated

- Alert prioritisation by severity and age
- Alert ownership and status management in a SIEM dashboard
- Investigating alerts using context and surrounding events
- True Positive / False Positive classification with written justification
