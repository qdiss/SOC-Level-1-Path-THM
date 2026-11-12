# Phishing Prevention - TryHackMe SOC Level 1

**Module:** Phishing Analysis  
**Date completed:** 2026-11-12  
**Room link:** https://tryhackme.com/room/phishingemails4gkxh  
**Path:** SOC Level 1

## Overview

This is the fourth room of the Phishing Analysis module. It covers the security controls organizations use to prevent, detect, and mitigate phishing: email authentication with SPF, DKIM, DMARC, and S/MIME, analysis of SMTP traffic, and the technical and user-facing defenses that reduce phishing risk. I practised on a PCAP file with SMTP traffic in a lab virtual machine using Wireshark.

## Objectives

- Understand core email security controls (SPF, DKIM, DMARC, S/MIME)
- Analyze SMTP network traffic and email content
- Explore anti-phishing protection measures

## Tools / Concepts Covered

- MITRE ATT&CK technique T1598, Phishing for Information
- SPF (Sender Policy Framework): record syntax, verification results, and intended actions
- DKIM (DomainKeys Identified Mail): private key signing, public key in DNS, record format
- DMARC: alignment of SPF and DKIM, policies (none, quarantine, reject), rua reporting tag
- S/MIME: digital signature (authentication, non-repudiation, integrity) and encryption (confidentiality)
- dmarcian tools: SPF Surveyor, DKIM Record Checker and Validator, Domain Checker
- Google Admin Toolbox Messageheader
- Wireshark with SMTP response code filters and the Internet Message Format (IMF) filter
- SMTP status codes
- Technical defenses: email filtering, secure email gateways, link rewriting, sandboxing
- User-facing defenses: warning indicators, phishing reporting, awareness training, phishing simulations

## Analysis Process

### Step 1 - SPF

I started with SPF, which authenticates the sender by checking that the sending mail server is authorized for the domain. An SPF record is a DNS TXT record listing the allowed senders, for example v=spf1 ip4:127.0.0.1 include:_spf.google.com -all, where v=spf1 starts the record, ip4 and include define who may send, and -all rejects everything else. The verification result decides the action: Pass, Neutral, and None are accepted, SoftFail and PermError are flagged as suspicious but allowed, and Fail and TempError are rejected. I used dmarcian's SPF Surveyor to look at TryHackMe's record, which lists no IPs directly but includes three authorized domains, and Messageheader to read an SPF softfail in a mock header.

### Step 2 - DKIM

DKIM adds a digital signature to each email. The sending server signs with a private key, and the receiving server fetches the public key from the domain's DKIM record in DNS to verify the signature. A sample record looks like v=DKIM1; k=rsa; p=public_key. Unlike SPF, DKIM survives forwarding. I read a spam header with a DKIM result of permerror, which is a permanent verification failure that can come from an invalid signature, a missing or incorrect DNS record, a forwarding server modifying the message, or a misconfiguration.

### Step 3 - DMARC

DMARC ties SPF and DKIM to the content of the email through alignment: the sender's domain has to match the domains verified by SPF and DKIM. If alignment fails, the policy in the DMARC record tells the receiving server what to do. A sample record is v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com, where p sets the policy and rua is where aggregate reports are sent. I used dmarcian's Domain Checker on microsoft.com, which passed all checks and uses the p=reject policy, the strictest one, which blocks emails that fail the check.

### Step 4 - S/MIME

S/MIME sends digitally signed and encrypted messages using public key cryptography. The signature, made with the sender's private key and verified with their public key, provides authentication, non-repudiation, and data integrity. Encryption with the recipient's public key provides confidentiality, so only the recipient can decrypt the message with their private key. I walked through the Bob and Mary example of exchanging certificates and public keys for future correspondence.

### Step 5 - Analysing SMTP Traffic

In the lab VM I opened traffic.pcap in Wireshark and filtered on SMTP response codes. I counted packets with the 220 Service ready response, found the response where an email was blocked by spamhaus.org and read its full response code message, and searched for code 552 to count the messages blocked for presenting potential security issues.

### Step 6 - Analysing Email Content with IMF

Using the same capture, I filtered on the Internet Message Format to examine the inside of emails: sender and recipient fields, content type, and attachments. I looked at the attachment in packet 270, the undeliverable message and the host IP that was not responding, the email client used to send a message with a suspicious attachment named attachment.scr, and the encoding used for that attachment.

### Step 7 - Technical and User-Facing Defenses

Technical controls stop phishing before it reaches users: email filtering by IP and domain reputation, secure email gateways that catch impersonation and spoofing, link rewriting that replaces suspicious URLs with safe redirected ones, and sandboxing that tests suspicious links and attachments in an isolated environment. Because some phishing will still get through, users need support too: warning banners such as External Sender, easy phishing reporting, awareness training, and phishing simulation exercises.

## Outcome / Classification

Completed the SPF, DKIM, DMARC, S/MIME, SMTP traffic analysis, and defense questions. The PCAP analysis identified blocked messages and a suspicious executable-style attachment (.scr) sent over SMTP.

## Key Takeaways

SPF, DKIM, and DMARC work as layers: SPF checks who may send, DKIM checks that the message was not altered, and DMARC enforces what happens when alignment fails. The p=reject policy gives the strongest protection. SMTP traffic in a PCAP shows what mail servers actually did with a message, including why it was blocked. Defense needs both technical controls and trained users, and sandboxing is the control for safely observing the behaviour of a suspicious attachment. This room connects to the earlier phishing analysis rooms, where I read the same SPF, DKIM, and DMARC results in headers.

## Skills Demonstrated

- Reading and interpreting SPF, DKIM, and DMARC records and results
- Using dmarcian tools and Messageheader to inspect authentication
- Explaining S/MIME signing and encryption
- Filtering SMTP response codes and IMF in Wireshark
- Identifying blocked messages and suspicious attachments in SMTP traffic
- Recognising technical and user-facing anti-phishing controls
