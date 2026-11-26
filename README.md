# SOC Analyst Portfolio - Adis Klobodanović

Documentation of my progress through the TryHackMe SOC Level 1 learning path, part of my transition from a software engineering background into cybersecurity, specifically toward a SOC Analyst role.

## About Me

I come from a software engineering background and I'm currently building hands-on security skills through TryHackMe, EC-Council, and Cisco Networking Academy while working toward a career as a SOC Analyst. This repository documents that process - writeups, notes, and reflections from each room/course I complete.

## Progress - TryHackMe

**SOC Level 1 - 14 modules**

- [x] **Blue Team Introduction**
- [x] **SOC Team Internals**
- [x] **Core SOC Solutions**
- [x] **Cyber Defence Frameworks**
- [x] **Phishing Analysis**
- [x] **Network Traffic Analysis**
- [x] **Network Security Monitoring**
- [x] **Web Security Monitoring**
- [ ] **Windows Security Monitoring** - *(in progress)*
  - [ ] Windows Logging for SOC
  - [ ] Windows Threat Detection 1
  - [ ] Windows Threat Detection 2
  - [ ] Windows Threat Detection 3
- [ ] **Linux Security Monitoring**
- [ ] **Malware Concepts for SOC**
- [ ] **Threat Analysis Tools**
- [ ] **SIEM Triage for SOC**
- [ ] **SOC Level 1 Capstone Challenges**

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
├── 14-capstone-challenges/
└── additional-training/
    ├── chatgpt-for-compliance/
    └── hands-on-pentesting-raspberry-pi-5/
```

Each numbered folder corresponds to a module in the TryHackMe SOC Level 1 path. Each room has its own subfolder with a detailed writeup covering the analysis process, tools used, and key takeaways - not answers or flags, in line with TryHackMe's guidelines.

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
- Wireshark packet analysis: statistics views, display filters, and advanced filter operators
- Wireshark fundamentals: packet dissection by layer, stream following, file export, and expert information
- Attack detection in packet captures: Nmap scans, ARP poisoning, Log4j, FTP brute force, user-agent anomalies, and HTTPS decryption
- Network forensics with NetworkMiner: host profiling, credential and file extraction from PCAPs
- Perimeter monitoring: reading firewall, WAF, VPN, and IDS logs and reconstructing an attack chain from reconnaissance to exfiltration
- Network discovery detection: external vs. internal scanning, horizontal and vertical port scans, ping sweep, TCP SYN, and UDP scans
- Data exfiltration detection over DNS, HTTP, FTP, and ICMP using Wireshark and Splunk
- MITM attack detection: ARP spoofing, DNS spoofing, and SSL stripping in packet captures
- Snort IDS/IPS: operating modes, PCAP investigation, and writing detection rules
- Web security fundamentals: request-response cycle, layered protection of application, web server, and host, and the roles of CDN, WAF, and antivirus
- Web attack detection: directory fuzzing, brute force, and SQL injection in access logs, confirmed in packet captures, plus writing basic WAF rules
- Web DDoS detection: access log indicators, Splunk statistics and timechart, and layered mitigation (CDN, WAF rate limiting, challenges, input validation)
- Web shell detection: Apache access logs, auditd correlation, file system hunting with find and grep, and Wireshark HTTP filters

## Tools

TryHackMe simulated SIEM environment, TryHackMe SOC Simulator, URL/IP reputation tools, simulated EDR console, Splunk, Elastic Stack (Kibana), CyberChef, Wireshark, NetworkMiner, Snort, Apache/Nginx access logs, auditd, Linux command line (grep, find, cut) *(updated as new tools are introduced in later modules)*

## Additional Training

- [ChatGPT for Compliance (EC-Council)](./additional-training/chatgpt-for-compliance/) - [Certificate](https://learn.eccouncil.org/certificate/a95a3b97-bd87-4bfa-a077-95a0dada4a0a)
- [Hands-on Penetration Testing with Raspberry Pi 5 (EC-Council)](./additional-training/hands-on-pentesting-raspberry-pi-5/) - [Certificate](https://learn.eccouncil.org/certificate/37d3680c-ef19-4ad5-a797-67fdab54d044)

## Contact

[LinkedIn](https://www.linkedin.com/in/klobodanovic-adis/) | [Email](mailto:klobodanovic.adis.21@size.ba)
