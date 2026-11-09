# MITRE - TryHackMe SOC Level 1

- **Module:** Cyber Defence Frameworks
- **Date completed:** 2026-10-28
- **Room link:** https://tryhackme.com/room/mitre
- **Path:** SOC Level 1

## Overview

This room introduces MITRE's cyber security frameworks and resources:
ATT&CK, the Cyber Analytics Repository (CAR), D3FEND, and newer
projects such as adversary emulation tools and frameworks for digital
assets and AI systems. I used them to profile threat groups, find
detection ideas, and map attacker techniques to defensive measures.

## Objectives

- Understand the purpose and structure of the MITRE ATT&CK framework
- See how security professionals apply ATT&CK in their work
- Use cyber threat intelligence and the ATT&CK Matrix to profile threats
- Discover MITRE's other frameworks, including CAR and D3FEND

## Tools / Concepts Covered

- ATT&CK tactics, techniques, sub-techniques, and procedures (TTPs) with technique IDs
- ATT&CK Matrix (Enterprise, Mobile, ICS) and ATT&CK Navigator
- Threat group pages, mitigations, and detection guidance
- CAR (Cyber Analytics Repository) with pseudocode and SIEM query examples
- D3FEND, the matrix of defensive techniques
- Adversary Emulation Library and Caldera
- AADAPT (digital asset systems) and ATLAS (AI and machine learning systems)

## Analysis Process

### Step 1 - Understanding ATT&CK

ATT&CK is a knowledge base of adversary behaviour based on real-world
observations. A tactic is the adversary's goal (the why), a technique
is how they achieve it, and a procedure is the specific implementation.
The matrix lays tactics across the top with techniques and
sub-techniques beneath, and each technique page has an ID,
description, example groups and software, mitigations, detections, and
references. Standard names and IDs make it easier for teams to compare
incidents and communicate clearly.

### Step 2 - How Teams Use ATT&CK

Threat intelligence teams map observed behaviour to TTPs to build
actionable profiles. SOC analysts link alerts to techniques for
context and prioritisation, detection engineers map rules to
techniques to find coverage gaps, incident responders map timelines to
the attack stages, and red and purple teams build emulation plans. I
practised this by analysing a threat group's page and its technique
matrix in the Navigator.

### Step 3 - Threat Profiling Scenario

Acting as an analyst in a sector migrating to the cloud, I used the
Groups section to find a relevant threat group, inspected which
techniques it uses, identified a technique of concern for cloud email
and productivity services, and then looked up related tools,
mitigations, and a detection strategy. This showed me how to go from
a sector-level question to concrete defensive actions.

### Step 4 - CAR and D3FEND

CAR provides ready-made detection analytics tied to ATT&CK techniques,
with pseudocode and example queries for tools like Splunk, which helps
turn a technique into a real SIEM detection. D3FEND is the defensive
counterpart: a matrix of countermeasures with its own tactics and
techniques, such as user behaviour analysis or credential rotation,
linked back to the digital artifacts and attack techniques they
address.

### Step 5 - Other MITRE Resources

I also reviewed the Adversary Emulation Library, which gives
step-by-step plans to emulate known groups, and Caldera, an
automated emulation tool for testing detections. Newer projects extend
the approach to other areas: AADAPT covers threats against digital
asset systems and ATLAS covers threats against AI and machine learning
systems.

## Outcome / Classification

Completed the questions on ATT&CK structure, threat group analysis,
the cloud-focused profiling scenario, CAR analytics, D3FEND, and the
additional MITRE frameworks.

## Key Takeaways

The main value of ATT&CK for a SOC analyst is a shared language. An
alert described as a technique ID with a tactic gives instant context
and points to what the attacker may do next, which connects directly to
the kill chain rooms I just completed. CAR and D3FEND complete the
picture by pairing each attacker technique with a way to detect it and
a way to defend against it, so I can move from understanding an attack
to improving coverage.

## Skills Demonstrated

- Navigating the ATT&CK matrix and Navigator to profile threat groups
- Mapping attacker behaviour to tactics, techniques, and sub-techniques
- Using CAR analytics and D3FEND techniques to plan detection and defence
- Using threat intelligence to assess defensive coverage gaps for a specific sector
