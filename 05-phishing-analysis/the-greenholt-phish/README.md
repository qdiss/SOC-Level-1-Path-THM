# The Greenholt Phish - TryHackMe SOC Level 1

**Module:** Phishing Analysis  
**Date completed:** 2026-11-12  
**Room link:** https://tryhackme.com/room/phishingemails5fgjlzxc    
**Path:** SOC Level 1

## Overview

This is the fifth room of the Phishing Analysis module and a challenge room that applies everything from the previous four. A sales executive at Greenholt PLC reported a suspicious email from a known customer: a generic greeting, an unexpected request for a money transfer, and an unsolicited attachment, none of which matched the customer's usual communication style. The message was escalated to the SOC, and my task was to analyse challenge.eml in a lab virtual machine and decide whether it was legitimate or a phishing attempt.

## Objectives

- Analyze the provided email to identify and extract key artifacts
- Investigate the message source to determine its origin and authenticity
- Use analysis tools to assess the potential maliciousness of the email

## Tools / Concepts Covered

- Email artifacts: subject line, display name, sender address, Reply-To address, originating IP
- Viewing raw message source in Thunderbird
- WHOIS lookup to find the owner of an IP address
- SPF and DMARC record lookups on the Return-Path domain
- Attachment extraction and hashing with sha256sum
- VirusTotal for file reputation, file size, and real file type
- Defanging indicators in reports
- Phishing red flags: generic greeting, unexpected money transfer request, unsolicited attachment, mismatch with normal customer behaviour

## Analysis Process

### Step 1 - Extracting Header Artifacts

I opened challenge.eml and started with the subject line, which contains the Transfer Reference Number 09674321. The sender's display name is Mr. James Jackson and the sender address is info@mutawamarine[.]com, but replies go to a different address, info.mutawamarine@mail[.]com, a free webmail account. A Reply-To that differs from the sender is a classic sign that the attacker wants responses to land in a mailbox they control.

### Step 2 - Analysing the Message Source

In the raw message source I found the originating IP address of the email: 192[.]119[.]71[.]157. This is the evidence that shows where the message really came from, rather than what the display name suggests.

### Step 3 - Investigating the Originating IP

A WHOIS search on 192[.]119[.]71[.]157 shows the IP is owned by HostPapa, a web hosting provider. A shared hosting IP is cheap and easy for an attacker to rent, which fits a phishing campaign better than a legitimate company's own mail infrastructure.

### Step 4 - Checking SPF and DMARC

I took the domain from the Return-Path field and ran an SPF check and a DMARC lookup.

- SPF: v=spf1 include:spf.protection.outlook.com -all
- DMARC: v=DMARC1; p=quarantine; fo=1

The SPF record authorizes only Microsoft 365 mail servers and rejects everything else (-all), but the email came from a HostPapa IP, so the sending server was not an authorized sender for the domain. The DMARC policy is quarantine, so mail that fails the check should be moved to spam rather than rejected, and fo=1 requests failure reports when any authentication check fails.

### Step 5 - Analysing the Attachment

The attachment is named SWT_#09674321____PDF__.CAB. The name suggests a PDF, but the extension is .CAB. I downloaded it only inside the lab virtual machine and generated its SHA256 hash with sha256sum, which means I never needed to open the file:

2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f

### Step 6 - VirusTotal Research

I searched the hash in VirusTotal. The file is 400.26 KB and its actual file type is RAR. So the attachment is a RAR archive disguised with a PDF-style name and a .CAB extension, which is a typical way to trick users and basic filters.

## Outcome / Classification

Completed all questions on the email artifacts, origin, SPF and DMARC records, and attachment analysis. I classified the email as phishing based on the generic greeting, the unexpected money transfer request, the Reply-To mismatch, the sending IP not authorized by the domain's SPF record, and the disguised RAR attachment.

## Key Takeaways

Real analysis rarely depends on one clue. The Reply-To address, the originating IP and its owner, the SPF and DMARC records, and the attachment hash each give a separate piece of evidence, and together they support a confident classification. The real file type matters more than the file name, and a hash lets me research an attachment without opening it. Indicators should be defanged in reports so they cannot be clicked by accident. This room pulls together the earlier four rooms of the module and connects to my SOC Simulator phishing scenario.

## Skills Demonstrated

- Extracting sender, Reply-To, subject, and originating IP from email headers and source
- Investigating an IP owner with WHOIS
- Checking SPF and DMARC records for a Return-Path domain
- Safely downloading and hashing an attachment with sha256sum
- Researching a file in VirusTotal: detections, size, and real file type
- Defanging indicators when documenting findings
- Classifying an email as phishing based on combined evidence
