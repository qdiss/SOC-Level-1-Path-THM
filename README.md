# Adis Klobodanović - SOC Analyst Portfolio

I'm a software engineer moving into security. This repository holds my work from the TryHackMe SOC Level 1 path: investigations, detection exercises, and case reports written the way I would write them on the job.

| | |
|---|---|
| **Target role** | Junior SOC Analyst (L1) |
| **Based in** | Zenica, Bosnia and Herzegovina. Looking for roles in Austria |
| **Languages** | Bosnian (native), English (C1), German (learning) |
| **Progress** | 10 of 14 SOC Level 1 modules done, module 11 in progress |
| **Evidence** | 2 SOC Simulator scenarios with results, 2 EC-Council course certificates |

[LinkedIn](https://www.linkedin.com/in/klobodanovic-adis/) | [Email](mailto:klobodanovic.adis.21@size.ba) | [TryHackMe profile](https://tryhackme.com/p/qdiss) | [Portfolio](https://adiss.dev)

## About Me

My background is in software engineering. I'm training for a SOC analyst role through TryHackMe, EC-Council courses, and Cisco Networking Academy. Working with Linux, logs, and scripting before this helps a lot with log analysis and investigations, and this repo is where I show how I use it.

## How This Repository Works

Every room or scenario has its own folder with a README writeup. They all use the same layout so they are quick to scan:

- **Overview and objectives:** what the exercise was about
- **Analysis process:** the steps I took and why, not only the commands
- **Outcome:** my verdict, such as true positive, false positive, or escalate
- **Takeaways and skills:** what I learned and what it shows I can do

I don't publish flags, answers, or credentials, as TryHackMe asks. The writeups are about my method and reasoning.

## Featured Work

| Writeup | What I did |
|---|---|
| [Phishing Unfolding (SOC Simulator)](./05-phishing-analysis/phishing-unfolding/) | Triaged 35 live alerts in Splunk while a phishing attack played out, found every true positive, and wrote case reports using the Five Ws |
| [MITM Detection](./07-network-security-monitoring/mitm-detection/) | Rebuilt a chained attack (ARP spoofing, DNS spoofing, SSL stripping) from a packet capture and wrote up the timeline |
| [Data Exfiltration Detection](./07-network-security-monitoring/data-exfiltration-detection/) | Found data leaving the network over DNS, HTTP, FTP, and ICMP by matching Wireshark captures with Splunk logs |
| [Web Shell Detection](./08-web-security-monitoring/web-shell-detection/) | Followed a web shell upload through Apache access logs, auditd, and file system searches |
| [Linux Threat Detection 2](./10-linux-security-monitoring/linux-threat-detection-2/) | Reconstructed a cryptominer infection (SSH brute force, discovery, persistence, malware transfer) from auth and auditd logs |

## SOC Simulator Results

| Scenario | Result | True positive rate | False positive rate | Alerts closed | Mean time to resolve |
|---|---|---|---|---|---|
| [Introduction to Phishing](./02-soc-team-internals/introduction-to-phishing/) | Passed on second attempt | 100% | 100% | 4 | about 2 min |
| [Phishing Unfolding](./05-phishing-analysis/phishing-unfolding/) | Passed | 100% | 80% | 35 | about 1 min |

I failed my first attempt at Introduction to Phishing because I marked one phishing email as harmless. In Phishing Unfolding I flagged four harmless alerts as real threats. Both writeups explain what I got wrong and what I changed afterwards.

## Skills

**SOC operations**
- Alert triage, prioritisation, ownership, and true/false positive classification
- Alert reporting with the Five Ws, escalation to L2, and incident communication
- Investigation workbooks, identity and asset lookups, and network diagrams
- SOC metrics (false positive rate, MTTD, MTTA, MTTR) and SLA basics

**Security tools**
- EDR: endpoint telemetry, process trees, detection and response actions
- SIEM: Splunk (log ingestion, SPL searches, timecharts) and Elastic Stack (KQL, Kibana dashboards)
- SOAR: orchestration, automation, and playbook design
- Snort IDS/IPS: operating modes, PCAP investigation, and writing rules

**Windows log analysis**
- Windows event logs: logons (4624/4625), user management events, and process creation
- Sysmon events for processes, files, registry, network, and DNS
- PowerShell history for tracking attacker commands

**Linux log analysis**
- Reading plain-text Linux logs in `/var/log` (`syslog`, `auth.log`, package manager logs) and filtering them with `grep`
- Understanding system calls and why auditd and EDRs depend on them, plus the limits of bash history as evidence
- Reading auditd rules and output (`PROCTITLE`, `SYSCALL`, `auid` vs `uid`) and searching by key with `ausearch`
- Detecting Initial Access: SSH brute force, command injection in Nginx logs, and human-led and supply chain attacks
- Walking a process tree up with `--pid` and listing child processes with `--ppid` to find the origin of an attack
- Spotting discovery commands and judging them in context
- Auditd and `ausearch` for process creation, with PID/PPID process tree tracing
- SSH brute force and successful logins in `auth.log`, and ingress tool transfer detection
- Reconstructing a cryptominer infection chain from Linux logs
- Detecting reverse shells, privilege escalation, and persistence (cron, systemd, new users, SSH keys)

**Threat frameworks and hunting**
- Pyramid of Pain, Cyber Kill Chain, and Unified Kill Chain
- MITRE ATT&CK, CAR, and D3FEND: mapping TTPs, profiling threat groups, planning detections
- Threat-informed hunting and purple-team detection engineering

**Phishing and email analysis**
- Email headers and bodies, attachment decoding, and defanging indicators
- Spoofing, link manipulation, tracking pixels, and credential harvesting
- Phishing kit analysis, file hash lookups (VirusTotal), and campaign scoping

**Network analysis**
- Wireshark: statistics, display filters, stream following, and HTTPS decryption
- Spotting Nmap scans, network discovery, ARP poisoning, Log4j, and FTP attacks in captures
- MITM detection and data exfiltration detection over DNS, HTTP, FTP, and ICMP
- NetworkMiner, NetFlow/IPFIX, and perimeter log analysis

**Web security monitoring**
- Access log analysis: directory fuzzing, brute force, and SQL injection
- Web shell detection with logs, auditd, file system searches, and network traffic
- DoS/DDoS detection and mitigation with CDN, WAF rate limiting, and challenges

**Compliance and AI**
- Using ChatGPT to support compliance work, with attention to its limits and risks

## Tools

TryHackMe SOC Simulator, Splunk, Elastic Stack (Kibana), Wireshark, NetworkMiner, Snort, Sysmon, Windows Event Viewer, auditd and ausearch, CyberChef, VirusTotal and other URL/IP reputation tools, a simulated EDR console, Apache/Nginx access logs, and the Linux command line (grep, find, cut). I add to this list as I learn new tools.

## Progress - TryHackMe SOC Level 1

Click a module to open it.

<details>
<summary><b>1. Blue Team Introduction</b> (completed)</summary>

- [x] Junior Security Analyst Intro
- [x] SOC Role in Blue Team
- [x] Humans as Attack Vectors
- [x] Systems as Attack Vectors

[Open writeups](./01-blue-team-introduction/)

</details>

<details>
<summary><b>2. SOC Team Internals</b> (completed)</summary>

- [x] SOC L1 Alert Triage
- [x] SOC L1 Alert Reporting
- [x] SOC Workbooks and Lookups
- [x] SOC Metrics and Objectives
- [x] Introduction to Phishing (SOC Simulator)

[Open writeups](./02-soc-team-internals/)

</details>

<details>
<summary><b>3. Core SOC Solutions</b> (completed)</summary>

- [x] Endpoint Detection and Response (EDR)
- [x] Splunk: Basics
- [x] Elastic Stack (ELK)
- [x] Security Orchestration, Automation, and Response (SOAR)

[Open writeups](./03-core-soc-solutions/)

</details>

<details>
<summary><b>4. Cyber Defence Frameworks</b> (completed)</summary>

- [x] Pyramid of Pain
- [x] Cyber Kill Chain
- [x] Unified Kill Chain
- [x] MITRE
- [x] Summit
- [x] Eviction

[Open writeups](./04-cyber-defence-frameworks/)

</details>

<details>
<summary><b>5. Phishing Analysis</b> (completed)</summary>

- [x] Phishing Analysis Fundamentals
- [x] Phishing Emails in Action
- [x] Phishing Analysis Tools
- [x] Phishing Prevention
- [x] The Greenholt Phish
- [x] Snapped Phish-ing Line
- [x] Phishing Unfolding (SOC Simulator)

[Open writeups](./05-phishing-analysis/)

</details>

<details>
<summary><b>6. Network Traffic Analysis</b> (completed)</summary>

- [x] Network Traffic Basics
- [x] Wireshark: The Basics
- [x] Wireshark: Packet Operations
- [x] Wireshark: Traffic Analysis
- [x] NetworkMiner

[Open writeups](./06-network-traffic-analysis/)

</details>

<details>
<summary><b>7. Network Security Monitoring</b> (completed)</summary>

- [x] Network Security Essentials
- [x] Network Discovery Detection
- [x] Data Exfiltration Detection
- [x] MITM Detection
- [x] Snort

[Open writeups](./07-network-security-monitoring/)

</details>

<details>
<summary><b>8. Web Security Monitoring</b> (completed)</summary>

- [x] Web Security Essentials
- [x] Detecting Web Attacks
- [x] Web Shell Detection
- [x] Detecting Web DDoS

[Open writeups](./08-web-security-monitoring/)

</details>

<details>
<summary><b>9. Windows Security Monitoring</b> (completed)</summary>

- [x] Windows Logging for SOC
- [x] Windows Threat Detection 1
- [x] Windows Threat Detection 2
- [x] Windows Threat Detection 3

[Open writeups](./09-windows-security-monitoring/)

</details>

<details>
<summary><b>10. Linux Security Monitoring</b> (completed)</summary>

- [x] Linux Logging for SOC
- [x] Linux Threat Detection 1
- [x] Linux Threat Detection 2
- [x] Linux Threat Detection 3

[Open writeups](./10-linux-security-monitoring/)

</details>

<details open>
<summary><b>11. Malware Concepts for SOC</b> (in progress)</summary>

- [x] Malware Classification
- [ ] Intro to Malware Analysis
- [ ] Living Off the Land Attacks
- [ ] Shadow Trace - Challenge 

[Open writeups](./11-malware-concepts/)

</details>

Not started yet:

12. Threat Analysis Tools
13. SIEM Triage for SOC
14. SOC Level 1 Capstone Challenges

## What I'm Working On

I'm in Malware Concepts for SOC now. After that come threat analysis tools, SIEM triage, and the capstone. Once I finish the path I plan to go for an entry-level SOC certification.

## Certificates and Additional Training

| Course | Provider | Status |
|---|---|---|
| [Hands-on Penetration Testing with Raspberry Pi 5](./additional-training/hands-on-pentesting-raspberry-pi-5/) | EC-Council | Completed, [certificate](https://learn.eccouncil.org/certificate/37d3680c-ef19-4ad5-a797-67fdab54d044) |
| [ChatGPT for Compliance](./additional-training/chatgpt-for-compliance/) | EC-Council | Completed, [certificate](https://learn.eccouncil.org/certificate/a95a3b97-bd87-4bfa-a077-95a0dada4a0a) |

## Repository Structure

```
├── 01-blue-team-introduction/
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
├── 14-capstone-challenges/
├── additional-training/
│   ├── chatgpt-for-compliance/
│   └── hands-on-pentesting-raspberry-pi-5/
└── WRITEUP-TEMPLATE.md
```

Each numbered folder is a module of the SOC Level 1 path. Each room inside has its own folder with a `README.md` writeup.

## Contact

I'm looking for a junior SOC analyst job. You can reach me here:

[LinkedIn](https://www.linkedin.com/in/klobodanovic-adis/) | [Email](mailto:klobodanovic.adis.21@size.ba)
