# Windows Threat Detection 2 - TryHackMe SOC Level 1

- **Module:** Windows Security Monitoring
- **Date completed:** 2026-11-27
- **Room link:** https://tryhackme.com/room/windowsthreatdetection2
- **Path:** SOC Level 1

## Overview

This room covers what attackers do after getting into a Windows host
when they want to act immediately instead of hiding: Discovery,
Collection, exfiltration, and downloading more tools. I ran sample
commands and malware in a lab VM and found each action in the Sysmon
logs.

## Objectives

- Detect common Discovery techniques with Windows event logs
- Trace where an attack started by building a process tree
- Learn what data attackers look for and how they take it out
- See how malicious commands are logged by running them myself

## Tools and Concepts

- MITRE Discovery, Collection, Credential Access, and Exfiltration tactics
- Sysmon process creation (event 1) and process trees from ProcessId and ParentProcessId
- Discovery with CMD and PowerShell commands, and through the graphical interface
- Collection targets: browser data, chat history, crypto wallets, SSH keys, databases
- Data stealers and exfiltration to cloud storage, code sites, messengers, and lookalike domains
- Ingress Tool Transfer with certutil, curl, and PowerShell Invoke-WebRequest

## What I Did

### 1. Discovery

Attackers want to know who they are, what the machine is for, which
apps and security tools it has, and whether it belongs to a corporate
network. They use built-in commands for users and groups, files, system
information, network settings, and the installed antivirus. When
someone has a graphical session, they may use tools like Computer
Management or Task Manager instead, which shows up as a different process
tree under explorer.exe. I ran commands myself and found them in Sysmon,
and then I ran a sample piece of malware and followed its discovery
commands, including its check for Microsoft Defender and where it sent
the information it gathered.

### 2. Detecting Discovery

I look for a sequence of discovery commands in a short time (Sysmon
event 1 or PowerShell history) and then check where they came from by
building the process tree. A single `ipconfig` is often an admin or a
legitimate tool, but a burst launched by a file from the Downloads
folder is a different story.

### 3. Collection and exfiltration

The targets depend on the goal: personal data for blackmail, wallets and
sessions for theft, or credentials and databases for corporate access.
In the VM I found the kinds of secrets a user leaves on a workstation,
such as saved browser passwords, SSH keys, and internal documents.
Humans collect with tools like Notepad, find commands, and 7-Zip for
archiving, which appear as new processes. Data stealers do the same
automatically, mostly with their own code and without CMD commands, so
the file and network activity matters more. I ran a stealer sample and
found the folder it created, the file types it searched for, how it read
the clipboard, and where it sent the data.

### 4. Ingress tool transfer

Attackers do not start with every tool, so they download more as they
need it. Common methods are certutil, curl, and PowerShell, or simply a
browser or copy and paste over RDP. I downloaded the same file with each
method and looked at the logs. The best detection is the network
connection or DNS request from the process, together with the
destination and the file written to disk. Attackers often use legitimate
services like GitHub, so I need to look at the process as well as the
domain.

## Result

Completed the questions on Discovery, Collection, the data stealer, and
tool transfer, tracing each activity in Sysmon.

## Takeaways

- Discovery usually comes right after Initial Access and Sysmon detects it.
- Discovery is about identifying the victim, and Collection is about
  getting valuable data.
- Attackers can exfiltrate data or download new tools at any moment.
- They download new malware only when they need it, so network events
  from unexpected processes matter.

## Skills

- Recognising discovery command sequences and judging them in context
- Building process trees from Sysmon event 1
- Identifying collection, archiving, and exfiltration activity
- Detecting ingress tool transfer with different download methods
