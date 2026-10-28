# SOC Metrics and Objectives - TryHackMe SOC Level 1

- **Module:** SOC Team Internals
- **Date completed:** 2026-10-28
- **Room link:** [https://tryhackme.com/room/socmetricsobjectives](https://tryhackme.com/room/socmetricsobjectives)
- **Path:** SOC Level 1

## Overview

This room explains how SOC performance is measured: the internal
metrics that describe alert quality and team reliability, and the
time-based metrics that usually form an SLA. It also covers why these
metrics matter to an L1 analyst and how to help improve them.

## Objectives

- Understand SLA, MTTD, MTTA, and MTTR
- Understand the importance of the False Positive rate
- Learn why and how an L1 analyst can improve the metrics
- Practice managing SOC performance metrics in a scenario-based exercise

## Tools / Concepts Covered

- Internal metrics: Alerts Count, False Positive Rate, Alert Escalation Rate, Threat Detection Rate
- Time-based metrics: MTTD, MTTA, MTTR
- Service Level Agreements (SLA) and SOC availability models (8/5 vs. 24/7)
- False Positive remediation through detection rule tuning and automation

## Analysis Process

### Step 1 - Internal Quality Metrics

I studied four metrics and what each says about the team:

| Metric | What it indicates |
|---|---|
| Alerts Count | Analyst workload; both too many and suspiciously too few are problems |
| False Positive Rate | Noise level; very high rates reduce analyst vigilance |
| Alert Escalation Rate | How independent and experienced L1 analysts are |
| Threat Detection Rate | Reliability of the SOC; the goal is always 100% |

A key insight was that "no alerts" is not automatically good news, as
it can point to a broken SIEM or missing visibility.

### Step 2 - Time-Based Metrics and SLAs

MTTD measures time from attack to detection, MTTA measures time until
an analyst starts triage, and MTTR measures time until the threat is
actually stopped. These are usually agreed in an SLA between the SOC
(or MSSP) and the business, and the SOC working schedule (8/5 vs.
24/7) directly affects acknowledgement time. I practiced calculating
all three from a realistic malware incident timeline.

### Step 3 - Improving the Metrics

For each problematic metric, the room gives practical fixes: tune
detection rules and automate common alerts for a high False Positive
rate, speed up log collection for slow detection, ensure real-time
notifications and even alert distribution for slow acknowledgement,
and escalate quickly with documented response steps for slow response.
As L1, I am often the first to notice these issues, so communicating
them to L2, engineers, and the manager is part of the job.

### Step 4 - Practical Scenarios

In the final exercise I acted as a SOC manager handling complaints and
improved the metrics in three scenarios by assigning the right
improvement tasks.

## Outcome / Classification

Completed the metric calculations and all three improvement scenarios.

## Key Takeaways

Metrics turn "the SOC is busy" into measurable problems with specific
fixes. They also affect me personally as an analyst, since good
results support growth toward L2. I now see that a high False
Positive rate or a slow acknowledgement time is not just an
inconvenience but a real security risk, because it increases the
chance that a genuine threat gets missed.

## Skills Demonstrated

- Calculating and interpreting SOC metrics (FPR, MTTD, MTTA, MTTR)
- Understanding SLAs and the impact of SOC availability models
- Identifying causes of poor SOC performance and proposing fixes
- Communicating operational issues such as alert noise and delays
