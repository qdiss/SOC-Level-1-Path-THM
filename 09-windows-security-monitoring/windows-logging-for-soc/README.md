# Windows Logging for SOC - TryHackMe SOC Level 1

- **Module:** Windows Security Monitoring
- **Date completed:** 2026-11-26
- **Room link:** https://tryhackme.com/room/windowsloggingforsoc
- **Path:** SOC Level 1

## Overview

This is the first room of the Windows Security Monitoring module. It
covers the Windows logs a SOC analyst uses every day: the Security log
(logons and user management), Sysmon for process and network activity,
and the PowerShell history file. I worked through sample event log
files in a lab VM and traced what an attacker did step by step.

## Objectives

- Find and read the important Windows event logs
- Learn the log sources that matter most, such as Sysmon and PowerShell
- Prepare to use these logs in SOC Simulator scenarios and later rooms
- Practise log analysis on several event log datasets

## Tools and Concepts

- Windows Event Viewer and EVTX files
- Security log events: 4624 (successful logon), 4625 (failed logon)
- User management events: account created, enabled, changed, disabled, password reset, and group membership changes
- Process creation logging with Security event 4688 and Sysmon event 1
- Other Sysmon events: file create, registry value set, network connection, and DNS query
- Correlating events with Logon ID and Process ID
- PowerShell history file (`ConsoleHost_history.txt`)

## What I Did

### 1. Reading Windows logs

Windows stores logs as binary EVTX files in the system logs folder, so
I used Event Viewer to open them. Each event has an ID, a timestamp
(in system time, not UTC), keywords such as success or failure, and a
details view with the full content. There are hundreds of event IDs,
and many are not enabled by default, so it is more useful to know the
handful that come up in almost every investigation.

### 2. Logons and RDP brute force

Events 4624 and 4625 are the starting point for most cases. A failed
logon shows brute force, password spraying, or scanning, and a
successful logon shows where the attacker got in. The fields I checked
were the logon type, the account name, the source IP and hostname, and
the Logon ID. In the sample Security log I found a burst of failed
logons from one IP, followed by a successful remote logon, which told
me who was attacked, which account was compromised, and which session
belonged to the attacker. 4624 is noisy on busy servers, and 4625 has
caveats, so the surrounding context matters more than a single event.

### 3. User management events

After getting in, attackers often create or change accounts to keep
access. The events for creating, enabling, changing, disabling, and
deleting accounts, resetting passwords, and adding or removing group
members all share the same structure: who did it (Subject), who it was
done to (Object), and what changed (Details). The Logon ID in the
Subject lets me connect the action back to the earlier logon event. In
the sample log I found a new account created shortly after the
malicious login and saw it added to privileged groups.

### 4. Process creation and Sysmon

Process monitoring is the most important log source for most
attacks. Security event 4688 records new processes but is disabled by
default and has few fields. Sysmon event 1 gives much more: process
path and command line, parent process, hashes and signatures, and the
user and Logon ID. I used it to follow a user's browsing session and
find which browser launched, what file was downloaded, and where it
came from.

### 5. Other Sysmon events

Sysmon also logs file creation, registry changes, network connections,
and DNS queries. These events share the same process fields but lack the
full context, so I matched them to the process creation event by
Process ID. With them I could see how the malware kept itself on the
host, where it connected, and which domain the address belonged to.

### 6. PowerShell history

Sysmon only shows that powershell.exe started, not what was typed
into it. The PowerShell history file records every command entered in
a user's console, survives reboots, and exists separately for each user,
so I checked more than one account. It does not show command output or
the content of scripts that are run from a file, so it is one source
among several, not a complete record.

## Result

I completed all the questions on the sample Security and Sysmon logs
and the PowerShell history, including finding the attacker's
logon, the account they created, and the activity of the downloaded
malware.

## Takeaways

- Learn 4624 and 4625 well, because they appear constantly in SOC work.
- Group events by Logon ID and Process ID to see the attack chain
  quickly.
- Sysmon gives far more context than the default logs and is worth
  setting up.
- Do not skip PowerShell logging, since attackers use it more every
  year.

This room ties together things from earlier modules: the Kill Chain
stages I studied show up here as concrete events (initial access as a
logon, persistence as a new account or registry change, command and
control as a network connection).

## Skills

- Reading and filtering Windows event logs in Event Viewer
- Investigating brute force and RDP logons with 4624 and 4625
- Tracking account creation and privilege changes
- Following process trees and activity with Sysmon
- Using the PowerShell history to see attacker commands
