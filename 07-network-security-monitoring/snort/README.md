# Snort - TryHackMe SOC Level 1

- **Module:** Network Security Monitoring
- **Date completed:** 2026-11-16
- **Room link:** https://tryhackme.com/room/snort
- **Path:** SOC Level 1

## Overview

This room introduces Snort, an open-source, rule-based network
intrusion detection and prevention system (NIDS/NIPS). I learned the
difference between IDS and IPS, the operating modes of Snort, how to
read and investigate packet captures with it, and how to write basic
detection rules. I practised in a lab VM with provided capture files
and a traffic generator script.

## Objectives

- Understand the overview of IDS and IPS
- Understand Snort and its capabilities
- Write IDS rules
- Detect intrusions with Snort

## Tools / Concepts Covered

- IDS vs. IPS and their types: NIDS, HIDS, NIPS, behaviour-based (NBA), WIPS, HIPS
- Detection techniques: signature-based, behaviour-based, policy-based
- Snort modes: sniffer, packet logger, and NIDS/NIPS
- Command-line parameters for sniffing, logging, reading, alert modes, and IPS (inline) mode
- Reading PCAP files with Snort and Berkeley Packet Filter (BPF) expressions
- Snort rule structure: action, protocol, addresses, ports, direction, and options
- Main components, rule types, `snort.conf`, and `local.rules`

## Analysis Process

### Step 1 - IDS vs. IPS and Snort Basics

An IDS passively monitors and alerts, while an IPS actively blocks or
terminates suspicious activity. Both can be network based or host
based, and behaviour-based systems need a baselining period to learn
normal traffic. Snort has three main uses: as a packet sniffer, a
packet logger, and a full network intrusion detection or prevention
system. I verified the installation, tested the configuration file,
and used quiet mode to suppress the banner.

### Step 2 - Sniffer and Logger Modes

In sniffer mode, different flags control how much of each packet is
shown, from basic headers to payload, link-layer headers, and a full
hex dump. In logger mode, Snort writes captured traffic either as a
binary file, which can be read back with Snort, tcpdump, or Wireshark,
or as human-readable ASCII files sorted into folders. Because Snort
runs as root, log files are root-owned and need privileges or an
ownership change to be read. I also filtered logs with BPF expressions
and limited the number of packets processed.

### Step 3 - IDS/IPS Mode and PCAP Investigation

With a configuration file and rules, Snort generates alerts in several
formats (console, cmg, fast, full, or none), and can run in the
background as a daemon. IPS mode requires inline operation with a DAQ
module and two interfaces so Snort can drop packets instead of just
alerting. I also processed PCAP files, including multiple files with
the option that shows which file generated which alert, and compared
results across different configuration files.

### Step 4 - Writing Snort Rules

A rule has a header (action, protocol, source and destination
addresses and ports, direction) and optional rule options. I practised
IP and port filtering, negation, ranges, and directions, along with
general options (message, SID, revision, reference), payload options
(content, nocase, fast_pattern), and non-payload options (IP ID, TCP
flags, payload size, same-IP). User-created rules go in `local.rules`
with SIDs in the user range, and each SID must be unique.

### Step 5 - Configuration and Components

I reviewed Snort's components (decoder, preprocessors, detection
engine, logging and alerting, output plugins), the three rule sets
(community, registered, subscriber), the DAQ modes, and the key
sections of `snort.conf` such as the network variables, decoder and
IPS settings, output plugins, and rule includes.

## Outcome / Classification

Completed the exercises on Snort's operating modes, PCAP analysis, and
rule writing, running my own rules against provided capture files and
checking the generated alerts.

## Key Takeaways

Snort's value depends on its rules and configuration, so understanding
the rule structure matters more than memorising commands. The
recommended habits are to build rules step by step, test them before
using them in production, comment out rules I no longer need instead of deleting
working rules, and back up configuration files before changing them.
Using Snort to replay a capture against known rules speeds up an
investigation, which complements the manual analysis I did earlier in
Wireshark.

## Skills Demonstrated

- Explaining the differences between IDS and IPS and their detection techniques
- Running Snort in sniffer, logger, IDS, IPS, and PCAP modes
- Writing and testing Snort rules for IPs, ports, payload content, TCP flags, and other options
- Reading Snort logs and alerts and filtering with BPF
- Understanding Snort's configuration file, components, and rule sets
