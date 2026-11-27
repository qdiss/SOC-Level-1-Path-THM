# Linux Threat Detection 3 - TryHackMe SOC Level 1

- **Module:** Linux Security Monitoring
- **Date completed:** 2026-11-27
- **Room link:** https://tryhackme.com/room/linuxthreatdetection3
- **Path:** SOC Level 1

## Overview

The last room of the Linux threat detection series looks at the
manual, targeted attacks that are closer to advanced Windows
intrusions than to the automated botnet infections from the previous
room. It covers reverse shells, privilege escalation, and the most
common ways attackers stay on a Linux system, and I traced each of them
in auditd and authentication logs in a lab VM.

## Objectives

- Learn how reverse shells are used in Linux intrusions
- Understand how attackers escalate privileges
- Explore the most common persistence techniques on Linux
- Find these techniques in system logs

## Tools and Concepts

- Reverse shells with bash, socat, and Python
- Auditd and `ausearch` for process trees (PID and PPID), file changes, and executed commands
- Privilege escalation through unpatched software, SUID binaries, and exposed keys or secrets
- Persistence with cron jobs, systemd services, new user accounts, and SSH keys
- Authentication logs for user creation and group changes
- Application-level persistence such as web shells
- Targeted Linux attacks: entry points, espionage, and ransomware on hypervisors

## What I Did

### 1. Reverse shells

Many exploits and web vulnerabilities only give attackers a limited
way to run commands, with timeouts, no proper terminal, and restrictions
on the network. A reverse shell fixes this by making the victim connect
back to the attacker and hand over a shell. The security team treats
it as a critical alert, because it means a human is already working in
the system. I tested a vulnerable command input in a lab web app,
and then found a similar reverse shell in exported auditd logs. The
method is to search for the suspicious command, such as socat or bash
with a network connection, and walk up the process tree until the
parent turns out to be the vulnerable application. After that I walked
down the tree to list the discovery commands the attacker ran from the
new shell.

### 2. Privilege escalation

A foothold through a web service usually has low privileges, so the
attacker needs to become root. There are too many techniques to detect
them one by one: outdated kernels, SUID binaries, readable private keys
or secrets. A better approach is to look at the events around the
escalation. In the example, the pattern was a burst of discovery
commands, a download into a temp folder followed by compile and run,
and then data archived and copied out as root. To confirm that an
escalation worked, I compared the user ID of the exploit process with
the user ID of the shell it started. In the exercise I used the same
approach to find the command that searched files for passwords, the
command that escalated to root, and what the attacker found.

### 3. Persistence

Cron jobs and systemd services keep malware running after a reboot.
Both are plain files, so I monitored the cron and systemd folders for
changes, along with the `crontab` and `systemctl` commands. For
persistent access without malware, attackers create a new account and
add it to the sudo group, which shows up in the authentication log
(`useradd` and `usermod` events), or they add their own key to a
user's `authorized_keys` file. Key changes are hard to catch with
process logs, because a shell builtin like `echo` is not logged as a
separate command, so I watched the file itself with auditd. In the lab I
found the malware persisted as a service and as a cron job, a created
backdoor account, and the file changed for SSH key persistence. I also
read about application-level persistence, such as a web shell,
which system logs may never record. If malware keeps coming back after
every system check, a public-facing application may be compromised.

### 4. Targeted attacks

Linux servers are often firewalls, web servers, and mail servers, so a
single compromised one can be the way into a mostly Windows network.
State-sponsored groups have used Linux backdoors with systemd persistence,
and ransomware increasingly targets hypervisors, where one Linux host
can put many virtual machines at risk.

## Result

I completed the questions on reverse shells, privilege escalation, and
persistence, and the exercises where I detected two persistence
methods through auditd and two more through account and SSH key
changes.

## Takeaways

- A reverse shell is a critical alert. Follow it up the process tree to
  find how the attacker got in, and down to see what they did next.
- Detecting privilege escalation works best by looking at the
  surrounding activity, and by comparing the user before and after.
- Monitor the files that persistence depends on: cron, systemd,
  `authorized_keys`, and user and group changes.
- Linux is often skipped in monitoring, which makes it a blind spot that
  attackers can use.

## Skills

- Detecting reverse shells and tracing them to the vulnerable application
- Recognising privilege escalation from surrounding events and user ID changes
- Finding cron, systemd, user account, and SSH key persistence with auditd and auth logs
- Building process trees with `ausearch` to follow an attacker's activity
- Explaining why Linux systems matter in targeted attacks and ransomware
