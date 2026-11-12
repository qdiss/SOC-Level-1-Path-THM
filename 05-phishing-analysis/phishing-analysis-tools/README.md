# Phishing Analysis Tools - TryHackMe SOC Level 1

**Module:** Phishing Analysis  
**Date completed:** 2026-11-12  
**Room link:** https://tryhackme.com/room/phishingemails3tryoe
**Path:** SOC Level 1

## Overview

This is the third room of the Phishing Analysis module. It builds on the fundamentals room by covering the tools that enable deeper email analysis: which artifacts to collect, how to check IP, URL, and file reputation, how to detonate attachments in malware sandboxes, and how PhishTool ties it together. I applied this by investigating three real-world phishing cases using a lab virtual machine and ANY.RUN sandbox reports.

## Objectives

- Understand what information should be collected during an email analysis
- Learn about the tools that enable effective email investigation
- Explore IP and URL analysis, including reputation lookups
- Investigate phishing cases to apply my understanding and strengthen my analysis skills

## Tools / Concepts Covered

- Header artifacts: sender email, sender IP (with reverse lookup), subject line, recipient (To/CC/BCC), Reply-To, date and time
- Body artifacts: URLs and hyperlinks, attachment names, attachment hash
- Header analysis tools: Google Messageheader, Message Header Analyzer
- IP and URL reputation: IPinfo, URLScan.io, Cisco Talos Reputation Center
- URL extraction with CyberChef and URL extractor tools
- Hashing attachments with sha256sum on Linux
- File and indicator reputation with VirusTotal
- Malware sandboxes: ANY.RUN, Hybrid Analysis, Joe Sandbox
- PhishTool for email analysis and case documentation
- Viewing raw message source in Thunderbird (View > Message Source)

## Analysis Process

### Step 1 - Collecting Key Artifacts

I started with what to collect before any deeper investigation. From the header: sender address, sender IP, subject line (urgency or a call to action), recipient, Reply-To, and date and time. From the body: URLs and hyperlinks (shortened ones need expanding to reveal the real destination), attachment names, and the attachment hash for threat intelligence lookups.

### Step 2 - Header Analysis Tools

Some details, such as the sender IP and Reply-To address, can only be found in the header. Instead of reading the source by hand, I pasted the full header into Google Messageheader and Message Header Analyzer, which extract the sender IP, routing path, authentication results such as SPF, and potential misconfigurations.

### Step 3 - IP and URL Reputation

I used IPinfo to see the location and organisation behind an IP, URLScan.io to inspect a website safely without visiting it directly (it records the session and takes a screenshot), and the Talos Reputation Center to check whether an IP, domain, or file hash has been tied to malicious activity.

### Step 4 - Analysing the Body and Attachments

Link destinations can be checked safely by copying the link address without following it, or by extracting every URL from the raw source with a URL extractor or CyberChef. Attachments should only be downloaded in a controlled environment such as a lab machine or sandbox. Once there, I generated the SHA256 hash with sha256sum and checked it in Talos and VirusTotal, where the sample file was labelled as phishing, malicious, and spam.

### Step 5 - Malware Sandboxes

I reviewed ANY.RUN, Hybrid Analysis, and Joe Sandbox. They run a suspicious file or URL in a controlled environment and show process activity, network connections, downloaded payloads, and other indicators of compromise, so I can understand what an attachment does without advanced malware analysis skills and without executing it myself.

### Step 6 - PhishTool

PhishTool shows the rendered HTML, raw HTML, and message source of an uploaded email, plus tabs for authentication results, transmission path, URLs, and attachments. Its VirusTotal integration shows reputation results without leaving the tool. At the end, I can mark the email as malicious, flag the key artifacts, add notes, and click Resolve, which mirrors case closure in a real SOC.

### Step 7 - Applying It to Three Cases

- Case 1: in the lab VM I opened Phish3Case1.eml in Thunderbird and read the header and body. I identified the impersonated brand, the intended recipient, the Received: from IP, a domain of interest from the Return-Path field, and the shortened URL behind the UPDATE ACCOUNT NOW button.
- Case 2: in ANY.RUN I analysed a fake Netflix payment update email with a malicious PDF attachment. I reviewed the classification, the attachment name and MD5 hash, the malicious URL in the Processes tab, and the IP its domain resolves to under Connections.
- Case 3: in ANY.RUN I analysed a malicious Excel attachment. I traced the suspicious process created after launch, the full path of the malicious binary, the CVE it tries to exploit, the domain a COVID-themed malware was downloaded from, and the associated network threat.

## Outcome / Classification

Completed the artifact, tools, and sandbox questions, and all three phishing cases. Each case was classified as malicious based on the evidence in its headers, URLs, attachments, and sandbox behaviour.

## Key Takeaways

Collecting the same artifacts every time (sender, IP, Reply-To, URLs, hashes) makes an investigation systematic instead of ad hoc. Reputation lookups and sandboxes let me understand a malicious file without ever running it on my own machine, and the IPs, domains, and URLs in these cases have hosted real malicious content, so they stay inside the lab. This room builds directly on Phishing Analysis Fundamentals and connects to my earlier SOC Simulator phishing scenario.

## Skills Demonstrated

- Collecting header and body artifacts from suspicious emails
- Analysing headers with Messageheader and Message Header Analyzer
- Checking IP, URL, and file reputation with IPinfo, URLScan.io, Talos, and VirusTotal
- Extracting URLs and hashing attachments with CyberChef and sha256sum
- Reading ANY.RUN sandbox reports: processes, network connections, IOCs, and CVEs
- Documenting and resolving a phishing case in PhishTool
