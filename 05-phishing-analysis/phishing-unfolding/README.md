# Phishing Unfolding (SOC Simulator) - TryHackMe SOC Level 1

- **Module:** Phishing Analysis
- **Date completed:** 2026-11-14
- **Platform:** TryHackMe SOC Simulator (https://tryhackme.com/soc-sim)
- **SIEM used:** Splunk
- **Difficulty:** Medium
- **Path:** SOC Level 1

## Overview

A SOC Simulator scenario where I followed a live phishing attack as it
unfolded inside a corporate network. Alerts arrived in real time while
the attack progressed, and my job was to triage them, separate true
positives from false positives, reconstruct the attack chain, and
document each stage in case reports.

## Objectives

- Monitor and analyse alerts as the attack unfolds
- Identify and document critical events such as PowerShell executions, reverse shell connections, and suspicious DNS requests
- Write detailed case reports that explain the full scope of the breach

## Tools / Concepts Covered

- SOC Simulator alert queue, ownership, and case report workflow
- Splunk for log searching and investigation
- A lab VM with analysis tools for deeper investigation
- Alert triage and prioritisation by severity
- Five Ws case reporting and escalation decisions
- Attack chain reconstruction from alerts (phishing, execution, command and control)

## Analysis Process

### Step 1 - Triage the Alert Queue

I started in the alert queue, prioritised the alerts that looked most
critical, and took ownership of one at a time. Ownership starts the
resolution timer, so I focused on working each alert efficiently. New
alerts kept appearing as the scenario progressed, which meant the
queue had to be re-prioritised continuously.

### Step 2 - Investigate with Splunk

For each alert I searched the related logs in Splunk to confirm what
actually happened: which user and host were involved, what process or
command ran, and what network connections followed. Where needed I
used the lab VM for deeper analysis of files and indicators.

### Step 3 - Reconstruct the Attack Chain

Individual alerts were connected into one timeline, following the
attack from the phishing delivery through code execution to outbound
connections to attacker infrastructure. Linking alerts to each other
mattered more than judging each one in isolation, because the later
alerts showed the impact of the earlier ones.

### Step 4 - Write the Case Reports

For each alert I classified it as a true or false positive and wrote a
report covering who was affected, what happened, when and where it
occurred, and why I reached that verdict, followed by recommended
remediation and a list of indicators. I decided on escalation based on
whether the threat was contained or had already reached the endpoint.

## Results

| Metric | Result |
|---|---|
| Scenario result | Passed (all true positives identified) |
| True Positive identification rate | 100% |
| False Positive identification rate | 80% |
| Closed alerts | 35 |
| Mean time to resolve | about 1 minute |
| Mean dwell time | about 4 minutes |

## Mistakes and Lessons Learned

I found every real threat, but four benign alerts were classified as
true positives, so my false positive identification was 80%. All four
were low-severity "suspicious email from external domain" alerts. I
treated an external sender as enough evidence and did not verify the
sender and link reputation before deciding. The lesson is that an
unfamiliar external domain is not a verdict. The email needs a
reputation check and a look at its content and links first, otherwise
I create unnecessary work for L2.

The AI feedback on my reports was also useful:

- **State the attacker's objective.** My reports described what
  happened in detail but did not always explain the motivation behind
  each action, which is what helps stakeholders understand intent.
- **Anchor each activity to its environment.** I should consistently
  state where an activity happened, especially for network resources
  and working directories, in relation to the wider infrastructure.
- **Speed versus accuracy.** The high-severity process alerts were
  closed very quickly and correctly, while the phishing alerts, where
  I made my mistakes, deserved more careful analysis rather than a
  fast verdict.

## Key Takeaways

This scenario was more demanding than a static lab because the attack
changed while I was investigating it. The main lesson is that good
triage means connecting alerts into a story: the phishing email, the
PowerShell execution, the reverse shell, and the suspicious DNS
requests were all stages of one intrusion. It also reinforced that
the quality of the written report matters as much as the verdict,
since another analyst must be able to understand the full scope from
it alone.

## Skills Demonstrated

- Real-time alert triage and prioritisation under time pressure
- Investigating phishing-related activity using Splunk
- Reconstructing an attack chain from related alerts
- Writing structured case reports using the Five Ws
- Making escalation decisions based on impact and containment
