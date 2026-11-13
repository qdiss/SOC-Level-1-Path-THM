# Snapped Phish-ing Line - TryHackMe SOC Level 1

- **Module:** Phishing Analysis
- **Date completed:** 2026-11-13
- **Room link:** https://tryhackme.com/room/snappedphishingline  
- **Path:** SOC Level 1

## Overview

This challenge room puts the whole Phishing Analysis module into
practice in one investigation. I acted as an IT team member at a
financial company after several employees reported the same suspicious
email and some had already entered their credentials. My task was to
work out the scope of the campaign, trace how the attacker operated,
and collect indicators for the response.

## Objectives

- Analyse the provided email samples to identify key artifacts
- Investigate phishing URLs to understand redirection
- Retrieve and examine the phishing kit used in the attack
- Use cyber threat intelligence tools to learn about the adversary
- Analyse the phishing kit to uncover additional indicators

## Tools / Concepts Covered

- Email source analysis (senders, recipients, attachments)
- Inspecting HTML attachments and following redirection to the phishing page
- Identifying brand impersonation on a fake login page
- Finding attacker-hosted files on an exposed web directory
- File hashing with sha256sum and lookups in VirusTotal
- Reviewing a phishing kit's contents and the script that collects credentials
- CyberChef for decoding data

## Analysis Process

### Step 1 - Triage the Email Samples

I reviewed the collected emails to see who received them, what lure
was used, and which sender address the adversary used across the
campaign. Comparing several samples showed that the messages were part
of one coordinated campaign aimed at multiple people, rather than
isolated emails.

### Step 2 - Follow the Attachment and Redirection

One email carried an attachment that, when examined, contained a
redirection URL. I extracted the root domain of the destination and
opened the page in the lab environment to see what the victim would
see. It was a login page impersonating a well-known service, designed
to capture credentials.

### Step 3 - Look for Exposed Attacker Files

Because attackers often leave their tooling on the same web server, I
checked the site for exposed directories. This led to the phishing kit
archive and to a log file with captured submissions, which showed the
scope of the compromise, including users who had submitted credentials
more than once.

### Step 4 - Analyse the Kit and Use CTI

I downloaded the archive into the lab VM and calculated its SHA256
hash, then looked the hash up in VirusTotal. The report showed
additional threat classifications beyond phishing and listed the
archive's contents. After extracting the kit, I examined the script
that processes submitted credentials and found the address the
attacker uses to collect them, which is a valuable indicator for
blocking and reporting.

### Step 5 - Decode the Final Artifact

The last step was to locate a hidden file on the phishing site and
decode its contents with CyberChef to complete the investigation.

## Outcome / Classification

Confirmed a credential-harvesting phishing campaign, identified the
sender and redirection infrastructure, retrieved and analysed the
phishing kit, and collected indicators such as the file hash and the
attacker's collection address.

## Key Takeaways

This room showed how much a single sample can reveal when followed all
the way through: the email leads to the redirect, the redirect leads to
the kit, and the kit leads to the attacker's collection point and
victim log. It also demonstrated why analysts should check for exposed
directories and always hash and look up recovered files. For incident
response, the findings translate directly into actions such as
blocking the sender and domains, resetting the passwords of affected
users, and reporting the infrastructure.

## Skills Demonstrated

- Correlating multiple phishing emails into a single campaign
- Tracing redirection chains and identifying brand impersonation
- Retrieving and safely analysing a phishing kit
- Hashing files and using VirusTotal for threat intelligence
- Extracting indicators of compromise and determining the scope of affected users
