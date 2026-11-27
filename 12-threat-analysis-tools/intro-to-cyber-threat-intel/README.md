# Intro to Cyber Threat Intel - TryHackMe SOC Level 1

**Module:** Threat Analysis Tools  
**Date completed:** 2026-11-27  
**Room link:** https://tryhackme.com/room/cyberthreatintel  
**Path:** SOC Level 1  

## Overview

This room turns Cyber Threat Intelligence (CTI) from an abstract discipline into everyday SOC tasks: enriching indicators, triaging alerts, and turning raw data into context that supports a decision. It covers the difference between data, information, and intelligence, the indicator types an L1 analyst meets, feeds and platforms, the CTI lifecycle (through a worked scenario), and the standards and frameworks used to describe and share intel. It ends with a hands-on exercise where I build a threat profile from a simulated security monitoring tool.

## Objectives

- Understand what threat intelligence is and why it matters to SOC analysts
- Know the threat intelligence lifecycle and the indicators to look for
- Become familiar with intelligence sharing through feeds and platforms

## Tools and Concepts

- Data, information, and intelligence, and enrichment as the step that turns one into the next
- IOC (Indicator of Compromise), IOA (Indicator of Attack), and TTP (Tactics, Techniques, and Procedures)
- Indicator types and where to enrich them: IP, domain, URL, file hash, email address, and local artefact
- Enrichment sources such as WHOIS, VirusTotal, Shodan, urlscan.io, URLhaus, Hybrid Analysis, MalShare, and MXToolbox
- Feeds (CSV, JSON, STIX, TAXII) versus platforms (MISP, OpenCTI)
- Sources of intel: internal telemetry, commercial services, OSINT (for example AbuseIPDB and URLhaus), and communities and ISACs
- Four classes of intel: strategic, tactical, operational, and technical
- The six-phase CTI lifecycle: direction, collection, processing, analysis, dissemination, and feedback
- Traffic Light Protocol (TLP: CLEAR, GREEN, AMBER, RED)
- STIX and TAXII (Collection and Channel sharing models)
- MITRE ATT&CK, MITRE D3FEND, the Cyber Kill Chain, and CVE, CVSS, and NVD

## What I Did

### 1. Why CTI matters in triage

CTI gives the context to decide which of many alerts is real danger. It helps an L1 analyst answer three questions early: who or what is behind an indicator, what they did in the past, and what the organisation should do now. I learned the three layers: data is an unprocessed observable (an IP and port), information adds factual annotation (who registered it and when), and intelligence is analysed information that answers "so what" (it belongs to a current C2, so block it). The L1 job is to enrich artefacts until they become intelligence, or show that they never will.

### 2. Indicator types and enrichment paths

Each indicator type has its own enrichment path, so it matters more to recognise the type than to memorise tools. I reviewed the first resources and typical IOA or TTP for each:

- IPs: WHOIS, VirusTotal, Shodan (repeated SSH failures map to password guessing, T1110.003)
- Domains: WHOIS age, passive DNS, urlscan.io (a surge of queries to a very new domain)
- URLs: URLhaus and sandboxes
- File hashes: VirusTotal, Hybrid Analysis, and MalShare (for example process injection, T1055)
- Email addresses: header analysis and breach lookups
- Local artefacts: registry Run keys, Sigma rules, and EDR prevalence queries

### 3. Feeds, platforms, and sources

A feed is a scheduled stream of indicators, and over-ingesting feeds without curation produces false positives and destroys trust. A platform such as MISP or OpenCTI stores indicators, tracks enrichment, maps relationships, and enforces sharing permissions, and it should be the single source of truth. I also learned to record where each indicator came from, because source credibility matters if an IOC later causes a business disruption.

### 4. The CTI lifecycle in a scenario

I followed an L1 analyst protecting a production PostgreSQL database through the full cycle:

1. **Direction:** turn a broad mandate into measurable intelligence questions
2. **Collection:** gather from a commercial feed, an OSINT source, an internal MISP, and a vendor report, and keep dated raw copies
3. **Processing:** normalise formats, deduplicate, correlate, tag with source, date, and TLP, and produce firewall and EDR action files
4. **Analysis:** check relevance (for example a Splunk search for local attempts), grade confidence by source agreement and local sightings, and decide to block, alert, or monitor
5. **Dissemination:** give each stakeholder (firewall team, endpoint team, platform, management) only the format and detail they need
6. **Feedback:** measure results against the original objectives and plan the next iteration

When the same IP arrived with two different TLP labels, the stricter label won, which prevents accidental disclosure.

### 5. Standards and frameworks

- **TLP** controls how widely intel may be shared, and the label must travel with the indicator into every platform and ticket note.
- **STIX** describes threat information in a machine-readable JSON schema, and **TAXII** is the set of APIs used to exchange it.
- **ATT&CK** gives a shared label for adversary behaviour, so a triage note can contain a technique ID that L2 and IR understand immediately.
- **D3FEND** maps defensive techniques, so a note can contain a mitigation as well as a diagnosis (for example for a DNS tunnelling alert).
- The **Cyber Kill Chain** places observed activity in a stage from reconnaissance to actions on objectives.
- **CVE, CVSS, and NVD** identify, score, and describe vulnerabilities.

Not every indicator should be shared: privacy law, NDAs, and the risk of tipping off an adversary can all restrict sharing.

### 6. Practical threat profile

In the static lab I used a simulated security monitoring tool to map a simplified attack by filling in the threat details (the source of the email and the file that was downloaded), until the threat profile was complete.

## Result

I completed all the questions in the room, including the lifecycle and framework questions and the threat profile exercise.

## Takeaways

- CTI is a front-line triage tool, and it is not only for intelligence specialists.
- Intelligence is analysed information that answers "so what", while raw indicators alone are only data.
- Enrich each indicator according to its type, and record the source and TLP label with it.
- Curate feeds before trusting them, and use a platform such as MISP or OpenCTI as the single source of truth.
- The lifecycle is a loop: feedback feeds the next direction phase.
- ATT&CK IDs and D3FEND controls make triage notes useful to the people who receive them.

I connected this to the Cyber Defence Frameworks module (Cyber Kill Chain and MITRE), and to the SOC Metrics and Phishing Analysis rooms, where indicators and reputation lookups were used for alert decisions.

## Skills

- Distinguishing data, information, and intelligence
- Choosing the right enrichment path for each indicator type
- Using IOCs, IOAs, and TTPs in triage notes with ATT&CK IDs
- Understanding feeds, platforms (MISP and OpenCTI), and intel sources
- Following the six phases of the CTI lifecycle
- Handling intel with TLP labels and understanding STIX and TAXII
- Using ATT&CK, D3FEND, the Kill Chain, and CVE, CVSS, and NVD in investigations
- Building a threat profile from monitoring data
