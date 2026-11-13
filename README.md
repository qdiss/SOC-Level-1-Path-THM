# SOC Analyst Portfolio - Adis Klobodanović

Documentation of my progress through the TryHackMe **SOC Level 1** learning
path, part of my transition from a software/DevOps background into
cybersecurity, specifically toward a SOC Analyst role.

## About Me

I come from a software/DevOps engineering background (Linux, Nginx,
PostgreSQL, cloud VMs) and I'm currently building hands-on security
skills through TryHackMe while working toward a career as a SOC
Analyst. This repository documents that process - writeups, notes, and
reflections from each room I complete.

## Progress

**SOC Level 1 - 14 modules**

- [x] Blue Team Introduction
  - [x] Junior Security Analyst Intro
  - [x] SOC Role in Blue Team
  - [x] Humans as Attack Vectors
  - [x] Systems as Attack Vectors
- [x] SOC Team Internals
  - [x] SOC L1 Alert Triage
  - [x] SOC L1 Alert Reporting
  - [x] SOC Workbooks and Lookups
  - [x] SOC Metrics and Objectives
  - [x] Introduction to Phishing (SOC Simulator)
- [x] Core SOC Solutions 
  - [x] Endpoint Detection and Response (EDR)
  - [x] Splunk: Basics
  - [x] Elastic Stack (ELK)
  - [x] Security Orchestration, Automation, and Response (SOAR)
- [x] Cyber Defence Frameworks 
  - [x] Pyramid of Pain
  - [x] Cyber Kill Chain
  - [x] Unified Kill Chain
  - [x] MITRE
  - [x] Summit
  - [x] Eviction
- [x] Phishing Analysis
  - [x] Phishing Analysis Fundamentals
  - [x] Phishing Emails in Action
  - [x] Phishing Analysis Tools
  - [x] Phishing Prevention
  - [x] The Greenholt Phish
  - [x] Snapped Phish-ing Line
  - [x] Phishing Unfolding
- [x] Network Traffic Analysis - *(in progress)*
  - [x]  Phishing Analysis Fundamentals
  - [ ] Wireshark: The Basics
  - [ ] Wireshark: Packet Operations
  - [ ] Wireshark: Traffic Analysis
  - [ ] Network Miner
- [ ] Network Security Monitoring
- [ ] Web Security Monitoring
- [ ] Windows Security Monitoring
- [ ] Linux Security Monitoring
- [ ] Malware Concepts for SOC
- [ ] Threat Analysis Tools
- [ ] SIEM Triage for SOC
- [ ] SOC Level 1 Capstone Challenges

## Repository Structure

```
├── 01-blue-team-introduction/
│   ├── junior-security-analyst-intro/
│   ├── soc-role-in-blue-team/
│   ├── humans-as-attack-vectors/
│   └── systems-as-attack-vectors/
├── 02-soc-team-internals/
├── 03-core-soc-solutions/
├── 04-cyber-defence-frameworks/
├── 05-phishing-analysis/
├── 06-network-traffic-analysis/
├── 07-network-security-monitoring/
├── 08-web-security-monitoring/
├── 09-windows-security-monitoring/
├── 10-linux-security-monitoring/
├── 11-malware-concepts/
├── 12-threat-analysis-tools/
├── 13-siem-triage/
└── 14-capstone-challenges/
```

Each folder corresponds to a module in the TryHackMe SOC Level 1 path.
Each room has its own subfolder with a detailed writeup covering the
analysis process, tools used, and key takeaways - not answers or
flags, in line with TryHackMe's guidelines.

## Skills Covered So Far
 
- SOC team structure, escalation paths, and analyst career progression
- Social engineering recognition (phishing, deepfakes, impersonation)
- Vulnerability vs. misconfiguration triage and remediation planning
- Alert prioritisation, ownership, and True/False Positive classification in a SIEM dashboard
- Alert reporting (Five Ws), escalation to L2, and incident communication
- Context lookups (identity/asset inventory, network diagrams) and investigation workbooks
- SOC performance metrics (FPR, MTTD, MTTA, MTTR) and SLA concepts
- Phishing alert triage, firewall correlation, and Five Ws case reporting (SOC Simulator)
- EDR concepts: endpoint telemetry, process tree analysis, detection and response actions
- Splunk fundamentals: architecture, log ingestion, and basic SPL searches
- ELK fundamentals: KQL searching, Kibana visualisations, and dashboards
- SOAR concepts: orchestration, automation, playbooks, and workflow design
- Indicator analysis using the Pyramid of Pain (hashes, IPs, domains, artifacts, tools, TTPs)
- Cyber Kill Chain phases and mapping attacker activity to attack stages
- Unified Kill Chain: classifying attacker activity across In / Through / Out phases
- MITRE ATT&CK, CAR, and D3FEND: mapping TTPs, profiling threat groups, and planning detections
- Purple-team detection engineering: using Pyramid of Pain levels to prioritise detections
- Threat-informed hunting: turning an APT group's ATT&CK techniques into monitoring priorities
- Email header and body analysis, attachment decoding, and defanging of indicators
- Phishing technique recognition: spoofing, link manipulation, tracking pixels, credential harvesting, malicious attachments
- Phishing kit analysis, file hash lookups (VirusTotal), and campaign scoping
- Network traffic analysis fundamentals: TCP/IP layer visibility, North-South vs. East-West flows, packet capture, and NetFlow/IPFIX
## Tools
 
TryHackMe simulated SIEM environment, TryHackMe SOC Simulator, URL/IP reputation tools, simulated EDR console, Splunk, Elastic Stack (Kibana), CyberChef *(updated as new tools are
introduced in later modules)*
 

## Contact

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Adis_Klobodanovi%C4%87-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/klobodanovic-adis/)
[![Email](https://img.shields.io/badge/Email-klobodanovic.adis.21%40size.ba-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:klobodanovic.adis.21@size.ba)
