# Summit - TryHackMe SOC Level 1

- **Module:** Cyber Defence Frameworks
- **Date completed:** 2026-11-10
- **Room link:** https://tryhackme.com/room/summit
- **Path:** SOC Level 1

## Overview

Summit is a purple-team challenge built around the Pyramid of Pain. I
acted as the defender at a fictional company that runs a threat
simulation and detection engineering engagement. An external
penetration tester repeatedly tried to execute malware samples on a
simulated internal workstation, and my job was to configure the
security tools so each sample was detected and prevented. Every time a
detection worked, the adversary had to change their approach, forcing
me to move higher up the pyramid.

## Objectives

- Detect and prevent a series of malware samples using the available security tools
- Follow the Pyramid of Pain's ascending order of indicators
- Increase the adversary's cost of operations until they are forced to give up
- Apply ATT&CK and Pyramid of Pain concepts from the preceding rooms in a practical exercise

## Tools / Concepts Covered

- The Pyramid of Pain levels: hash values, IP addresses, domain names, host and network artifacts, tools, and TTPs
- Iterative purple-team workflow (attacker action, defender detection, attacker adaptation)
- Malware sample analysis to extract indicators
- Configuring detection and prevention controls in a simulated security environment
- Mapping attacker behaviour to MITRE ATT&CK techniques

## Analysis Process

### Step 1 - Working in a Purple-Team Loop

The exercise is iterative. The tester runs a sample, I analyse what it
did, identify an indicator, and configure the security tools to detect
or block it. The tester then modifies the malware to evade that
specific control, and I repeat the process with a new sample. Each
cycle is a practical demonstration of the Pyramid of Pain: indicators
that are easy for the attacker to change give only short-lived
protection.

### Step 2 - Starting at the Bottom of the Pyramid

The first samples were handled with the cheapest, most precise
indicators. These are fast to configure but trivial for an adversary
to change, which is exactly why the tester's next sample bypassed
them. This reinforced what the Pyramid of Pain room explained in
theory: blocking a single hash, address, or domain barely slows down a
motivated attacker.

### Step 3 - Moving Up to Artifacts, Tools, and Behaviour

As the samples changed, simple indicators stopped being enough, so I
shifted to indicators that were harder for the adversary to alter:
traces left on the host and on the network, and ultimately the
behaviour and techniques the malware relied on. Detecting at these
higher levels meant the adversary had to rebuild their tooling or
change their method entirely.

### Step 4 - Final Outcome

After the last sample was detected and prevented, the simulated
adversary was forced to give up, which completed the engagement. Each
successful detection returned a flag confirming the control worked.

## Outcome / Classification

All simulated malware samples were detected and prevented, and the
adversary was driven off after the detections reached the upper levels
of the Pyramid of Pain.

## Key Takeaways

This challenge made the Pyramid of Pain feel practical rather than
theoretical. Every detection I built at a low level lasted exactly one
iteration, while detections tied to behaviour and techniques held up
against changes in the malware. As a SOC analyst or detection
engineer, I should prefer rules that target how an attacker operates
over rules that target what a single sample looks like, while still
using fast, low-level blocks as a first response. It also showed the
value of the purple-team loop, where defenders get immediate feedback
on whether a control really works.

## Skills Demonstrated

- Extracting indicators from malware activity and choosing the right control for each
- Applying the Pyramid of Pain to prioritise detection quality over quick blocks
- Working in an iterative purple-team workflow
- Linking detections to attacker behaviour and ATT&CK techniques
