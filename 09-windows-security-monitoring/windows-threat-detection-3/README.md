# Windows Threat Detection 3 - TryHackMe SOC Level 1

- **Module:** Windows Security Monitoring
- **Date completed:** 2026-11-27
- **Room link:** https://tryhackme.com/room/windowsthreatdetection3
- **Path:** SOC Level 1

## Overview

The last room of the Windows threat detection series covers what
happens when attackers want to stay: Command and Control, Persistence,
and the Impact stage. I analysed sample logs in a lab VM for each of
the main persistence methods, from backdoor accounts to services,
scheduled tasks, startup folders, and Run keys.

## Objectives

- Review the concept of Command and Control (C2)
- Learn why and how attackers keep control of victims
- Use Windows event logs to find persistence methods
- See the techniques working in a hands-on environment

## Tools and Concepts

- MITRE Command and Control, Persistence, and Impact tactics
- Security events: user created (4720), added to a group (4732), password reset (4724)
- Service creation (Security 4697, System 7045) and scheduled task creation (Security 4698)
- Sysmon events for process creation (1), file creation (11), and registry value set (13)
- Process parents that give away persistence: `services.exe` and the task scheduler service
- Active Directory and ransomware as the final goal of many intrusions

## What I Did

### 1. Command and Control

Some attacks do not need C2, for example when the attacker types
commands in an RDP session. Otherwise, the first process of an attack,
such as a phishing attachment, connects back to the attacker. A more
careful attacker has it download a separate C2 program, hides it in a
folder like `C:\Temp`, and starts it as a new process, so the attack
continues even if the original file is deleted. In Sysmon I found the
suspicious download, where the C2 file was hidden, and the domain it
contacted.

### 2. Persistence through an exposed service

If RDP was the way in, the attacker can keep using it. To make it more
reliable they create a new user, add it to a privileged group, or reset
the password of an old account. I traced all three in the Security log:
the failed logons, the successful login, the new account, and the group
it was added to. Names alone are unreliable, so I ask who created the
account, from which IP, and what else happened in that session.

### 3. Persistence through malware

Malware needs to survive a reboot, and Windows offers many ways. I
covered four common ones:

- **Services:** created with `sc.exe`, visible in 4697 and 7045, and
  their processes have `services.exe` as the parent.
- **Scheduled tasks:** created with `schtasks.exe`, logged as 4698, and
  the processes run under the task scheduler service. They are the most
  common method because they are easy to configure and hide.
- **Startup folder:** a new file in the startup folder (Sysmon 11), which
  runs when the user logs in.
- **Run keys:** a new registry value in the Run key (Sysmon 13), also
  running at login.

In the lab, the attackers left several backdoors and restarted the
system, and I found each one with the Security and Sysmon logs, then
identified the parent process and the behaviour of the malware that
started from them.

### 4. Why attackers persist

Reasons include adding the host to a botnet, long-term espionage, and
using the host as an entry point into a network that may take weeks to
breach. In corporate Windows networks, the biggest danger is Active
Directory compromise leading to ransomware, which can stop an entire
organisation. Stopping the attack earlier is always better than later.

## Result

Completed the questions on C2, backdoor accounts, services, scheduled
tasks, startup folders, and Run keys, using the Security and Sysmon logs.

## Takeaways

- Every persistence method leaves a log event, usually a new service,
  task, file, registry value, or account.
- The parent process helps tell persistence from normal activity.
- Persistence is a signal that the attacker plans to stay, so it needs
  a fast response.
- The three rooms together cover the attack from breach to long-term
  access, and give me the events to look for at each stage.

## Skills

- Detecting new accounts, group changes, and password resets
- Finding service and scheduled task persistence in Security and Sysmon logs
- Detecting startup folder and Run key persistence
- Using parent processes and event sequences to confirm persistence
- Explaining why ransomware and Active Directory compromise are the main risks
