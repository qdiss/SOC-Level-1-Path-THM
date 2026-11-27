# Linux Logging for SOC - TryHackMe SOC Level 1

**Module:** Linux Security Monitoring  
**Date completed:** 2026-11-27  
**Room link:** https://tryhackme.com/room/linuxloggingforsoc  
**Path:** SOC Level 1  

## Overview

This room is the foundation for investigating Linux alerts and incidents as a SOC analyst. It explains where Linux keeps its logs, how to read and filter them directly on the host, and why default logging is not enough to see runtime events like process creation. It finishes with system calls and auditd, the tool used to monitor and report those events.

## Objectives

- Explore authentication, runtime, and system logs on Linux
- Learn the commands and pitfalls when working with logs
- Uncover how tools like auditd monitor and report the events
- Practise every log source in the attached VM

## Tools and Concepts

- Plain-text logs in `/var/log` (`syslog`, `kern.log`, `dpkg.log`, `apt`, `auth.log`)
- `cat`, `head`, `grep`, `grep -v`, and `grep -R` for filtering and discovering logs
- Authentication log (`/var/log/auth.log`, or `/var/log/secure` on RHEL-based systems)
- Application logs such as Nginx `access.log`
- Bash history (`~/.bash_history` and the `history` command) and its limitations
- System calls (for example `execve`) as the base of all runtime monitoring
- Auditd rules (`/etc/audit/rules.d/`), `/var/log/audit/audit.log`, and `ausearch -i -k <key>`
- Alternatives to auditd: Sysmon for Linux, Falco, Osquery, and EDRs

## What I Did

### 1. Log format and filtering

Linux logs most events into plain text files, so any text editor works and no tool like Event Viewer is needed. The downside is that the logs have no event codes and no strict format. I started with `/var/log/syslog`, filtered it with `grep CRON` (and `grep -v CRON` to exclude it), and used `grep -R -E "auth|login|session" /var/log` to discover which files hold login events. I also kept in mind that logging on different distributions can be customised, so the logs may look different or not exist at all.

### 2. Generic system logs and bash history

I checked the syslog for the time server used for clock sync and the kernel message from Yama, then used the package manager logs to see which version of a package was installed. Other common logs are `kern.log` for kernel messages, `dpkg.log` and `apt` on Debian-based systems, and `dnf.log` or `yum.log` on RHEL-based systems. They are useful in DFIR but are noisy and rarely used in the daily SOC routine. I also read the users' bash history on the VM and learned why it is unreliable: attackers can add a leading space to a command, hide commands inside a script, or switch to a shell like `/bin/sh` that does not save history. It also does not record non-interactive commands from cron jobs, services, or web servers.

### 3. Application logs

Application logs show what a specific program did: database queries, mail activity for phishing, container anomalies, and web requests. I looked at the Nginx access log format, where each line is a request with source IP, time, method, path, status code, and size.

### 4. Authentication logs

`auth.log` also holds user management events, sudo commands, and more. I filtered it for:

- Logins and logouts with `session opened` / `session closed` (local, SSH, SMB, cron, and `sudo`)
- SSH-specific events with `sshd` and `Accepted|Failed`, in the format `<result> <auth-method> for <user> from <ip>`
- User management with `passwd|useradd|usermod|userdel` to catch things like a new backdoor user added to the `sudo` group
- Commands run with sudo using `COMMAND=`, which can show actions like stopping an EDR service or reading firewall state

In the lab I found the IP that failed to log in as several users over SSH, and the user that was created and added to the `sudo` group.

### 5. Runtime monitoring, system calls and auditd

By default Linux does not log process creation, file changes, or network events. Every program that opens a file or creates a process has to use a system call (like `execve`), and all modern EDRs and logging tools monitor system calls. Auditd works from rules that define which calls to monitor, and `ausearch -i -k <key>` shows the results readably. A process event is split into `PROCTITLE`, `CWD`, `EXECVE`, and `SYSCALL` lines, and the important fields are:

- `pid` and `ppid` to link events and build a process tree
- `auid` (the original login account) and `uid` (the user who ran the command)
- `tty` to separate sessions
- `exe` for the executed binary
- `key` to filter by rule

I used `proc_wget` and `file_thmsecret` keys, and `grep` on the logs, to track a threat actor: when a secret file was first opened, which file was downloaded from GitHub with `wget`, and which network range was scanned with the downloaded tool (no dedicated key for that, so I had to find it in the raw auditd logs).

## Result

I completed all the questions in the room, covering syslog, package manager logs, bash history, authentication logs, system calls, and auditd process and file events.

## Takeaways

- Linux logging can be chaotic, but it often stores enough details to detect the threat.
- Logs are kept in `/var/log/` by default and are usually stored in plain text.
- The top three log sources for SOC are `auth.log`, app-specific logs, and runtime logs.
- Bash history is unreliable for SOC; auditd or an alternative is a better choice.
- Programs cannot bypass system calls, which is why monitoring them is so effective.

I connected this to the Windows Logging for SOC room: auditd plays the same role for Linux that Sysmon plays for Windows.

## Skills

- Reading and filtering plain-text Linux logs with `grep`
- Discovering which log files hold the events I need
- Investigating SSH logins, user management, and sudo activity in `auth.log`
- Understanding system calls and why runtime monitoring depends on them
- Reading auditd output and searching it with `ausearch` keys
- Knowing the limits of bash history as evidence
