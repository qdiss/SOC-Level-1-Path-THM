# Windows Threat Detection 1 - TryHackMe SOC Level 1

- **Module:** Windows Security Monitoring
- **Date completed:** 2026-11-27
- **Room link:** https://tryhackme.com/room/windowsthreatdetection1
- **Path:** SOC Level 1

## Overview

This room covers how attackers first get into a Windows machine
(Initial Access) and how to detect each method using only Windows event
logs. I went through exposed RDP, phishing attachments, and infected
USB drives, using sample log files in a lab VM.

## Objectives

- Understand how threat actors access and breach Windows machines
- Learn common Initial Access techniques from real-world examples
- Practise detecting each technique with Windows event logs

## Tools and Concepts

- MITRE Initial Access techniques: External Remote Services, Exploit Public-Facing Application, Phishing, Removable Media
- Security events 4624 and 4625 and logon types for remote logons
- Sysmon process creation (event 1) and file creation (event 11)
- Logon ID correlation between Security and Sysmon logs
- Phishing attachment types: executables, double extensions, and LNK shortcuts

## What I Did

### 1. Two groups of Initial Access

Methods that need an exposed service (RDP, a vulnerable web or mail
server) and methods that need a user to do something (opening a
phishing attachment, plugging in a USB). Large ransomware groups have
used all of them in real campaigns.

### 2. Detecting an RDP breach

An exposed RDP port with a weak password gets brute forced quickly. In a
sample Security log I filtered for failed logons (4625) with remote
logon types and external source IPs, which showed the brute force.
Switching to successful logons (4624) showed which account was breached
and which IP got in. Then I took the Logon ID of the interactive RDP
session and searched Sysmon for events with the same ID, which listed
everything the attacker ran. I also looked at the workstation name in
the logon event for more information about the attacker's machine.

### 3. Phishing attachments

Attackers use executable types users do not think of as dangerous
(.com, .scr, .cpl) and double extensions such as `invoice.pdf.exe`,
which look harmless because Windows hides known extensions by default.
LNK shortcuts can hide a PowerShell command that downloads the next
stage, and they leave very little trace of their own. I opened three
sample attachments in the VM and then followed the Sysmon event chain for
one of them: the browser starts, the archive is created in Downloads, the
file is extracted, and the user launches it from explorer.exe. For LNK
files, the earlier file creation event is the evidence.

### 4. Removable media

USB infections bypass the firewall, and most still depend on the user
double-clicking a file, such as a fake shortcut, a binary with a folder
icon, or a double-extension copy of a document. In the Sysmon log I
found which file was launched from the drive, what the malware dropped
on the disk, and which other USB it spread to. The detection looks a lot
like phishing, because both start from explorer.exe, so execution from
an external drive path is the key clue.

## Result

Completed the questions on the RDP, phishing, and USB scenarios,
tracing each attack from the first event to the malware's activity.

## Takeaways

- Exposed services and user-driven attacks are the two main Initial
  Access paths on Windows.
- An RDP breach can be found with the default authentication logs
  (4624 and 4625).
- User-driven attacks are best found through process execution events,
  and Sysmon gives the most detail.
- Each method, such as LNK files, has its own features that are learned
  best by practice.

## Skills

- Detecting RDP brute force and a successful breach in Security logs
- Correlating Security and Sysmon logs with the Logon ID
- Recognising malicious attachments, double extensions, and LNK payloads
- Following Sysmon event chains for phishing and USB infections
