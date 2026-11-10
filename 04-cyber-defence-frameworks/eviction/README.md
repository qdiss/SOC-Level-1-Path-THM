# Eviction - TryHackMe SOC Level 1

- **Module:** Cyber Defence Frameworks
- **Date completed:** 2026-11-10
- **Room link:** https://tryhackme.com/room/eviction
- **Path:** SOC Level 1

## Overview

Eviction is a scenario-based room where I played a SOC analyst at a
manufacturing company that received intelligence about an APT group
possibly targeting similar organisations. The task was to use the MITRE
ATT&CK Navigator layer for that group to identify its tactics and
techniques, work out what evidence to look for in the network, and
ultimately stop the group from stealing the company's intellectual
property.

## Objectives

- Use the ATT&CK Navigator to understand the TTPs of a specific APT group
- Follow the attack through its stages, from reconnaissance to exfiltration
- Decide which techniques, tools, and artefacts to look for at each stage
- Practice turning threat intelligence into a concrete hunting and detection plan

## Tools / Concepts Covered

- MITRE ATT&CK Navigator and group technique layers
- Attack stages: reconnaissance, resource development, initial access, execution, persistence, defence evasion, discovery, lateral movement, collection, command and control, exfiltration
- Social engineering and user execution
- Scripting interpreters, registry-based persistence, and system binary proxy execution
- Network discovery tooling and remote services used for lateral movement
- Proxy-based command and control techniques

## Analysis Process

### Step 1 - Starting from Threat Intelligence

The scenario begins with an intelligence report about a specific APT
group. Rather than waiting for an alert, I used the group's ATT&CK
layer to see which techniques it is known to use. This is a proactive
use of threat intelligence: the technique list tells the analyst what
to hunt for before the group is confirmed to be in the network.

### Step 2 - Following the Attack Through the Matrix

I worked through the group's techniques in the order an intrusion would
unfold. For each stage I asked what the technique means in practice
and where evidence would appear: which accounts or infrastructure the
group prepares, how a user might be tricked into running code, which
interpreters and registry locations should be monitored for execution
and persistence, which system binaries may be abused to evade
defences, and which discovery tools or remote services suggest
internal movement.

### Step 3 - Identifying the Target and Stopping Exfiltration

The group's goal was to steal intellectual property, so I identified
the type of information repository it would most likely target. I then
looked at the command and control stage and the proxy techniques the
group could use to hide exfiltration traffic, which showed where a
defender could cut the attack off even after data had been collected.

## Outcome / Classification

Completed the full attack-path analysis of the APT group using the
ATT&CK Navigator and identified the techniques to monitor at each
stage, which prevented the simulated theft of company data.

## Key Takeaways

A technique list from ATT&CK is only useful if it is translated into
something observable: a log source, a registry path, a process, or a
network pattern. Working through the stages in order also showed how
detections at one point can catch an attack that was missed earlier,
for example by spotting suspicious execution after a successful social
engineering attempt or by blocking command and control traffic before
exfiltration. This room tied together the earlier Cyber Defence
Frameworks rooms by using ATT&CK, the kill chain, and the Pyramid of
Pain thinking in one practical investigation.

## Skills Demonstrated

- Using ATT&CK Navigator layers to profile an APT group
- Translating techniques into monitoring and hunting priorities
- Tracing an intrusion from initial access to exfiltration across the ATT&CK stages
- Identifying detection and containment opportunities at each stage
