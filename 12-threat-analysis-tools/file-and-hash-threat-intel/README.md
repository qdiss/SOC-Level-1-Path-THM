# File and Hash Threat Intel - TryHackMe SOC Level 1

**Module:** Threat Analysis Tools  
**Date completed:** 2026-11-27  
**Room link:** https://tryhackme.com/room/fileandhashthreatintel  
**Path:** SOC Level 1  

## Overview

Every SOC alert follows three steps: verify, enrich, and decide. This room covers the enrich step for files. Starting from only a file path, name, or hash, I built context about a suspicious binary using filename heuristics, file hashes, VirusTotal, MalwareBazaar, and sandbox reports. The scenario is a time-boxed triage: an EDR flags several binaries on workstations, and I have to show within 60 minutes whether the files are bait, benign, or malicious.

## Objectives

- Interpret suspicious file paths and file names using heuristics
- Use VirusTotal and MalwareBazaar to enrich newly observed binaries
- Extract behaviour from sandbox telemetry and map it to MITRE ATT&CK

## Tools and Concepts

- File path analysis (`C:\Users\Public`, `C:\Windows\Temp\`, `C:\ProgramData\`) and filename heuristics
- File hashing with `certutil -hashfile`, `Get-FileHash`, and `sha256sum`
- VirusTotal: detection score, threat labels, detection rules, properties, relations, and behaviour
- MalwareBazaar: family tags, YARA rules, campaign attribution, and sample availability
- Sandbox reports from Hybrid Analysis and Joe Sandbox, and mapping behaviour to MITRE ATT&CK
- TryDetectThis, an offline tool that holds the vendor data used in the lab

## What I Did

### 1. File path and filename heuristics

File paths are the earliest heuristic an analyst has. They do not prove malice on their own, but they show attacker tradecraft. Common staging locations are `C:\Users\Public`, `C:\Windows\Temp\`, and `C:\ProgramData\`. I also learned the filename tricks that should trigger an investigation:

- **Double extensions**, like `invoice.pdf.exe`, which rely on hidden extensions
- **System binary impersonation**, like `scvhost.exe`, where the defence is to allowlist legitimate locations and not only names
- **High-entropy names**, like `jh8F21.exe`, which suggest automated packing or polymorphic generation
- **Masquerading**, like `backup-2300.exe` or single-character substitutions

In the lab I reviewed the case files and found the one that showed one of these indicators.

### 2. Hashing and VirusTotal

Attackers rename malware all the time, but the file hash identifies a file regardless of its name. I generated SHA256 hashes on Windows and Linux and followed some good practices: store hashes in lowercase, hash both an archive and the file extracted from it, record where and when I saw a hash, and remember that any byte change changes the hash. In VirusTotal I learned to read each section with its red flags:

- **Detection score and labels:** the number of reliable vendors that flag the file, and conflicting classifications
- **Upload time:** new malware often starts with low detection, so recheck after 24 hours
- **Signatures:** invalid or unrelated certificates, but also stolen valid ones
- **Properties:** odd compile times and high entropy, which can also come from legitimate packers
- **Relations:** known bad IPs and DGA-like domains, which I can check further in Shodan
- **Behaviour:** registry changes or process injection, which I correlate with endpoint logs

### 3. Cross-referencing with MalwareBazaar

MalwareBazaar adds malware family tags, YARA rules, campaign attribution, and downloadable samples. A file with only a few VirusTotal detections but a known family tag should still be treated as malicious. YARA rules from submissions can go into EDR or SIEM for hunting. I used TryDetectThis, which combines the VirusTotal and MalwareBazaar data, to find the hash, classification label, and first submission time of a suspicious file, and to review a second, non-malicious file and the ATT&CK technique flagged for it.

### 4. Dynamic analysis with sandboxes

Sandboxes let me confirm that a file actually executes, extract runtime IOCs, and map behaviour to ATT&CK. Hybrid Analysis gives behaviour trees and an ATT&CK heatmap that suits a quick summary, and Joe Sandbox goes deeper into system calls, strings, and memory. I read the report for a malicious file (tags, the stealth command line, and the spawned process) and analysed a file that masqueraded as a legitimate Windows file, finding the associated URL and the number of extracted strings. I also reviewed the limitations of sandboxes:

- Evasion through environment awareness and anti-debugging checks
- Short execution time that misses multi-stage or time-delayed malware
- Encrypted TLS traffic and DNS tunnelling that hide the payload
- Fileless and living-off-the-land malware that never touches disk

### 5. Practice challenge

I investigated a suspicious sample using TryDetectThis: its SHA256 hash, family labels, submission time, the file dropped during execution, the PowerShell command observed, and the MITRE ATT&CK ID connected to that command.

## Result

I completed all the questions in the room, covering filename heuristics, hash enrichment, VirusTotal and MalwareBazaar analysis, sandbox reports, and the practice sample.

## Takeaways

- Validate the evidence first: confirm that you have the exact binaries and hashes.
- Paths and names give quick context, but they are not proof on their own.
- Generate hashes early to pivot into external and internal knowledge bases.
- Read VirusTotal as a whole (score, labels, timing, signatures, properties, relations, and behaviour), and recheck new files later.
- Do not trust a single sandbox result, because evasion, short run times, and encrypted traffic cause false negatives.
- Turn findings into a short brief: indicators by type, behaviour, and recommendations supported by evidence.

I connected this to Intro to Malware Analysis (hashing and Hybrid Analysis) and to the Phishing Analysis module (file hash lookups with VirusTotal).

## Skills

- Judging suspicious file paths and filenames
- Calculating SHA256 hashes on Windows and Linux
- Reading VirusTotal results critically
- Using MalwareBazaar for family tags, YARA rules, and campaign attribution
- Reading sandbox reports and mapping behaviour to MITRE ATT&CK
- Understanding the limits of sandbox analysis
- Writing up file findings as indicators and proportionate recommendations
