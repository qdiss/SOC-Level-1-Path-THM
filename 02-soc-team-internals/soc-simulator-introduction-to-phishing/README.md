# Introduction to Phishing (SOC Simulator) - TryHackMe SOC Level 1

- **Module:** SOC Team Internals
- **Date completed:** 2026-10-28
- **Platform:** TryHackMe SOC Simulator [(https://tryhackme.com/soc-sim)](https://tryhackme.com/soc-sim/scenario-onboarding)
- **Path:** SOC Level 1

## Overview

A hands-on SOC Simulator scenario where I worked as an L1 analyst
triaging a queue of phishing-related alerts. The goal was to close all
True Positive alerts correctly, classify benign ones as False
Positives, write case reports, and decide when to escalate.

## Objectives

- Triage phishing email alerts and related firewall alerts
- Classify each alert as True Positive or False Positive
- Write structured case reports using the Five Ws
- Decide whether each alert requires escalation to L2

## Tools / Concepts Covered

- SOC Simulator alert queue and case report workflow
- URL / IP reputation lookups
- Phishing indicators (typosquatted domains, shortened URLs, urgency, brand impersonation)
- Correlating email alerts with firewall events
- Five Ws reporting and escalation decisions

## Analysis Process

### Step 1 - Triage the Email Alerts

For each phishing alert I checked the sender domain, the subject and
tone of the message, and the link. Typical red flags were look-alike
domains (character substitution), shortened URLs hiding the real
destination, and artificial urgency such as account warnings or
delivery deadlines. I ran suspicious URLs through the built-in URL/IP
reputation tool before deciding a verdict.

### Step 2 - Separate True from False Positives

Not every email flagged by the detection rule was malicious. Emails
that came from legitimate, expected senders with no suspicious link
behaviour were closed as False Positives, with a short explanation of
why the activity was benign.

### Step 3 - Correlate with Firewall Activity

One phishing email had a related firewall alert for an internal host
connecting to the same destination. I linked the two alerts, noted the
firewall action, and treated them as one incident rather than two
unrelated events.

### Step 4 - Write the Report and Decide on Escalation

Each True Positive got a case report with the affected entities, the
reasoning for the verdict, recommended remediation, and a list of
attack indicators. I then decided on escalation based on whether the
threat was contained or the user had potentially interacted with it.

## Results

| Metric | First attempt | Final attempt |
|---|---|---|
| True Positive identification rate | 67% | 100% |
| False Positive identification rate | 100% | 100% |
| Scenario result | Failed | Passed |
| Mean time to resolve | ~2 min | ~2 min |
| Mean dwell time | ~4 min | ~2 min |

## Mistakes and Lessons Learned

On my first attempt I closed one phishing email as a False Positive
when it was actually malicious, which failed the scenario. I replayed
it and passed, but the feedback on my reports was just as valuable
as the score:

- **Check the scope.** I did not always verify whether other users
  received mail from the same sender or connected to the same
  destination. Searching logs for the same indicators across all
  users and hosts is required before concluding the scope.
- **Attribute hosts to users.** An IP address is not enough. I should
  match it against the asset inventory and name the user and
  workstation in the report.
- **Use the evidence already available.** I sometimes recommended
  "review the logs" when those logs were already accessible. The
  report should state what the logs actually show.
- **Escalate based on impact.** A blocked connection with no other
  affected users points to a contained threat, while a user who
  reached a credential-harvesting page calls for immediate
  escalation and a definitive password reset.
- **Explain the attacker's goal.** A report should say what the
  malicious page is for (for example, harvesting credentials), not
  only that it is malicious.

## Key Takeaways

The simulator showed me the gap between knowing the triage steps and
doing them consistently under time pressure. Correct classification
is only part of the job. Reports must name the affected user, state
the scope based on evidence, and justify the escalation decision. The
failed first attempt was useful because it showed exactly where my
process was incomplete.

## Skills Demonstrated

- Phishing email analysis and URL reputation checks
- True Positive / False Positive classification
- Correlating email, firewall, and asset data across related alerts
- Writing structured case reports using the Five Ws
- Making escalation decisions based on containment and user impact
