# Linux Threat Detection 1 - TryHackMe SOC Level 1

**Module:** Linux Security Monitoring  
**Date completed:** 2026-11-27  
**Room link:** https://tryhackme.com/room/linuxthreatdetection1  
**Path:** SOC Level 1  

## Overview

This room covers how Linux breaches usually start. It uses the log sources from the Linux Logging for SOC room to detect common Initial Access techniques: exposed SSH, vulnerable public services, and human-led or supply chain attacks. Its most important skill is process tree analysis, which traces a suspicious command back to the application or user that started it.

## Objectives

- Understand the role and risk of SSH in Linux environments
- Learn how Internet-exposed services can lead to breaches
- Use process tree analysis to identify the origin of an attack
- Practise detecting Initial Access techniques in realistic labs

## Tools and Concepts

- SSH as Initial Access (MITRE T1133, External Remote Services)
- Brute force of weak passwords, stolen or leaked SSH keys, and botnets scanning the Internet
- Authentication log (`/var/log/auth.log`) with `grep "sshd"` and `grep -E 'Accepted'`
- Exploiting public services (MITRE T1190), for example command injection in a web app
- Web logs (`/var/log/nginx/access.log`)
- Auditd and `ausearch -i -x <command>`, `--pid`, and `--ppid` for process tree analysis
- Human-led attacks (`curl ... | bash`, typosquatted packages) and supply chain compromise (MITRE T1195)

## What I Did

### 1. SSH as Initial Access

Nearly every Internet-facing Linux machine has SSH enabled, and many still allow password logins with weak passwords. Attackers get in through stolen keys (from a repository, an automation server, or a data stealer on an admin's laptop) or through breached passwords. I used `grep "sshd"` on `auth.log` to find the first SSH login of the `ubuntu` user and checked whether it used a key or a password.

### 2. Detecting SSH attacks

Listing the successful logins with `grep -E 'Accepted'` is a simple starting point. A login from an internal IP using a public key at exactly the same time of day looks like an automation account (Ansible), while a password login from an external IP at night is a red flag. To make a verdict I also have to check the owner of the user, the reputation of the source IP, the login history before it, and what the user did next. In the lab I found when the SSH password brute force started, the four users the botnet tried to breach, and the IP that finally got into the root user.

### 3. Linux and public services

Web servers, mail servers, databases, VPNs, and firewalls running on Linux can all lead to a full host compromise (T1190), as in the Zimbra, exposed Docker API, and Palo Alto cases. Application logs rarely say that an exploit is happening, but they leave useful artifacts. I analysed the Nginx logs of a ping web app (TryPingMe) where the attacker put commands like `;whoami` and `;ls` into the `host` parameter, which is a command injection. From the web logs I found the Python file the attacker tried to open and the flag inside it.

### 4. Building the process tree

Application logs are not always available, so most SOC teams rely on process tree analysis. I found the suspicious command with `ausearch -i -x whoami`, then walked up the tree with `ausearch -i --pid <ppid>` until I reached the web application that started it. After that I listed the other children of the app with `ausearch -i --ppid <pid> | grep proctitle` to find more dangerous commands such as `curl ... | sh`. In the lab I found the PPID of `whoami`, the PID of the TryPingMe app, and the program used to open the reverse shell.

### 5. Human-led attacks and supply chain

Phishing and USB attacks are rare on Linux, but people can still run a shady script like `curl https://... | bash`, or mistype a package name and install a malicious one (`pip3 install fastpi`). Supply chain compromise, such as the XZ Utils backdoor or the tj-actions breach, infects many users through one trusted software. A trusted app that suddenly runs malicious commands points to supply chain compromise. All of these techniques can be detected with process tree analysis, starting from a trigger like a suspicious command or a connection to a known malicious IP, and tracing which application or user started it.

## Result

I completed all the questions in the room, covering SSH logins and brute force, web log analysis, process tree tracing with auditd, and the Initial Access techniques quiz.

## Takeaways

- Attacks on SSH are widespread, but they are easy to detect in authentication logs.
- Exposed services are always a risk since they can lead to a whole Linux compromise.
- Phishing is not common on Linux, but human-led and supply chain attacks are still possible.
- Process tree analysis is the best approach for finding out how an attack started.
- Context decides whether an event is malicious: who logged in, from where, using what method, and at what time.

I connected this to the Linux Logging for SOC room: `auth.log`, application logs, and auditd are the three sources I used here. The T1133 and T1190 techniques also match the Initial Access stage of the MITRE and Cyber Kill Chain rooms.

## Skills

- Identifying malicious SSH logins and brute force patterns in `auth.log`
- Judging a login by user, source IP, auth method, and time
- Spotting command injection in web server logs
- Building a process tree with `ausearch` using PID and PPID
- Listing child processes of a breached application to find malicious commands
- Recognising human-led and supply chain Initial Access techniques
