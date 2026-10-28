# SOC L1 Alert Reporting - TryHackMe SOC Level 1

- **Module:** SOC Team Internals
- **Date completed:** 2026-10-28
- **Room link:** [https://tryhackme.com/room/socl1alertreporting](https://tryhackme.com/room/socl1alertreporting)
- **Path:** SOC Level 1

## Overview

This room continues from alert triage and covers what an L1 analyst
does after reaching a verdict: writing a clear alert report,
escalating to L2 when needed, and communicating with other teams. It
is practiced in a simulated SOC dashboard with phishing and data
leakage alerts.

## Objectives

- Understand why SOC alert reporting and escalation are needed
- Learn how to write alert comments and case reports properly
- Explore escalation methods and communication best practices
- Apply them to alerts in a simulated environment

## Tools / Concepts Covered

- Alert reporting using the Five Ws approach (Who, What, When, Where, Why)
- Escalation criteria and escalation steps (reassigning to L2 with a verdict)
- Requesting L2 support
- Communication with other departments and crisis communication cases
- TryHackMe SOC dashboard simulation

## Analysis Process

### Step 1 - Why Reports Matter

A good report gives L2 the context to start without redoing the
analysis, preserves findings for the records (raw SIEM logs are kept
for months, alerts much longer), and improves my own investigation
skills, since explaining an alert simply requires understanding it.

### Step 2 - Writing the Report with the Five Ws

I structured my analyst comments around Who (user, host, sender),
What (the exact action or sequence), When (start and end of the
activity), Where (device, IP, or site involved), and Why, the
reasoning behind my verdict, which is the most important part. I
practiced this on a phishing alert and a data leakage alert.

### Step 3 - Deciding to Escalate

I learned the common escalation criteria: a likely major attack
needing deeper investigation or DFIR, remediation actions such as
host isolation or password reset, communication with customers,
management, or law enforcement, or simply not understanding the alert
well enough. In the dashboard I escalated by setting my verdict,
assigning the alert to the L2 on shift, and relying on the report as
the handoff.

### Step 4 - Communication in Unusual Situations

The room covered edge cases and the right response: if L2 does not
respond for a long time on a critical alert, move up through L3 and
then the manager; do not verify a compromised chat account through
that same chat; when overwhelmed, keep prioritising and keep L2
informed; if I realise I misclassified an alert, tell L2 immediately;
and if logs are not searchable, do not skip the alert, report the
problem to L2 or a SOC engineer.

## Outcome / Classification

Wrote Five Ws reports for two alerts and escalated them to L2 using
the correct workflow (verdict set, correct assignee, detailed
comment).

## Key Takeaways

Triage is only half of the L1 job. Without a clear report, an
escalation wastes L2's time, and without correct escalation a real
threat can sit unhandled. I also took away that asking for senior
help is expected, especially early on, and is far better than quietly
closing an alert I do not understand.

## Skills Demonstrated

- Writing structured alert reports using the Five Ws
- Deciding when to escalate and handing off to L2 with full context
- Communicating safely and appropriately in incident scenarios
- Handling edge cases such as unavailable L2, missed detections, and unparsed logs
