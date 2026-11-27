# Linux Threat Detection 2 - TryHackMe SOC Level 1

- **Module:** Linux Security Monitoring
- **Date completed:** 2026-11-27
- **Room link:** https://tryhackme.com/room/linuxthreatdetection2
- **Path:** SOC Level 1

## Overview

This room follows what attackers do once they are inside a Linux
system. It covers the first discovery commands they run, how malware
gets onto the victim, and the "hack and forget" attacks that are run
at scale by botnets. It ends with the full analysis of a real
cryptominer infection using auditd and authentication logs in a lab VM.

## Objectives

- Identify discovery commands in logs
- Learn the common threats to Linux servers
- Understand how attackers upload malware to a victim
- Practise by investigating a real cryptominer attack from start to finish

## Tools and Concepts

- Discovery commands (OS, users and groups, processes, network, virtualisation and sandbox checks)
- Specialised discovery for stealing data, mining cryptocurrency, and finding new victims
- Auditd and `ausearch` for process creation events, and process tree tracing with PID and PPID
- Authentication logs (`/var/log/auth.log`) for SSH logins
- Ingress tool transfer with wget, curl, and SSH-based file copy (scp/sftp)
- "Hack and forget" goals: cryptominers, botnets, and proxies
- Persistence by replacing SSH keys and changing passwords
- Indicators in temporary folders, hidden directories, and background processes (`nohup`)

## What I Did

### 1. Discovery commands

Most Linux breaches start with an automated botnet getting in, and a
human or script then looks around. The first commands are almost always
the same: where am I, who am I, what is running, and what does the
network look like. Because they are the same commands an admin would
use, the context decides whether it is an attack. A web server
suddenly running `whoami` is a red flag, and a monitoring tool pinging
the local network is normal. I practised running some of these on
the VM and checking which processes revealed security tooling.

### 2. Finding the origin of a command

With auditd I searched for a discovery command, then used the process
ID and parent process ID to trace it back to the script that started
it, and then listed everything else that script launched. In the
exercise this showed that a single alert for one command was part of a
script hunting for secrets, and the script itself contained
information about its author. Building the process tree is how I
separate a malicious script from legitimate activity.

### 3. Specialised discovery

After the initial look around, the commands show the goal. Searching for
credentials and keys points to data theft, reading CPU and memory
information points to a cryptominer, and scanning the internal network
for open ports points to a botnet looking for more victims.

### 4. Ingress tool transfer

Attackers download malware with wget or curl, or copy it with scp
over SSH. Downloads and local copies show up in auditd process logs,
but when the attacker pushes a file to the victim from their own
machine, there is no command on the victim, only a new SSH login in the
authentication log. I therefore check both sources. Other indicators
are connections to known bad or unusual domains, new files in temporary
folders, and antivirus or EDR alerts.

### 5. Cryptominer infection case

I analysed a well-known cryptominer campaign (Dota3). A botnet brute
forces SSH on exposed hosts, then the attacker runs discovery commands
that check CPU and memory, which makes the goal clear. For persistence
the attacker changes the account password and replaces the SSH keys,
which locks out the owner and competing botnets. The malware is copied
in with scp, unpacked into a hidden folder in `/tmp` with a name
designed to look legitimate, and then a network scanner and the miner
are started with `nohup` so they keep running after the session ends.
Using the lab logs, I traced the successful brute force, the commands
the attacker ran, the archive that was transferred, the cryptominer
command line, and which network range was scanned.

## Result

I completed all the questions in the discovery and cryptominer
exercises, including tracing a command back to its parent script and
reconstructing the infection chain from the auth and audit logs.

## Takeaways

- "Hack and forget" attacks are automated and run at scale by botnets.
- Almost every stage uses commands already on the system (ls, cat,
  wget, ssh), so detection depends on context and not on the command
  alone.
- Auditd and process tree analysis are the best tools for finding
  malicious commands, and the SSH authentication log covers what
  auditd cannot see.
- A weak SSH password is still how most of these infections begin.

I connected this to earlier rooms: it follows the Cyber Kill Chain
stages I studied (initial access, discovery, persistence, command and
control, impact) and uses the same log-correlation thinking as the
Windows logging room.

## Skills

- Recognising discovery commands and judging them in context
- Tracing a command to its parent script with auditd and PID/PPID
- Investigating SSH brute force and successful logins in `auth.log`
- Detecting ingress tool transfer through process logs, SSH logins, and file events
- Reconstructing a cryptominer infection chain from Linux logs
