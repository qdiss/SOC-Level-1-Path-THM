# IP and Domain Threat Intel - TryHackMe SOC Level 1

**Module:** Threat Analysis Tools  
**Date completed:** 2026-11-27  
**Room link:** https://tryhackme.com/room/ipanddomainthreatintel  
**Path:** SOC Level 1  

## Overview

Most SIEM and EDR alerts contain at least one IP address or domain, so enriching them is a core L1 skill. This room follows the `verify → enrich → decide` process for network indicators. I pivoted on DNS records, WHOIS age, ASNs, geolocation, exposed services, TLS certificates, and VPN detection to decide whether an indicator is malicious and to get more context about the attack. Because live intel changes quickly and attacker infrastructure is short-lived, the lab used exported reports and screenshots.

## Objectives

- Enrich domains with WHOIS age, DNS records, and TLS details
- Learn the concepts of ASNs and geolocation for SOC triage
- Spot red-flag services using VirusTotal, Shodan, and Censys
- Detect VPN, proxy, and Tor exit nodes with IP2Proxy and Spur
- Correlate signals across sources instead of trusting one verdict

## Tools and Concepts

- DNS records: A/AAAA and TXT (SPF and DKIM), with `nslookup.io` and `dnschecker.org`
- WHOIS and RDAP for registrar, registration date, and domain age
- DNS-based attack techniques: CDN abuse, typosquatting, and IDN homograph attacks (checked with Punycode)
- IP reputation with AbuseIPDB and VirusTotal, including community comments
- Autonomous Systems and ASNs with BGP.Tools, and geolocation with `ipinfo.io` and `iplocation.net`
- Exposed services with Shodan and Censys
- TLS certificate analysis with `crt.sh`, Censys, and SSL Shopper (issuer, validity, subject)
- VPN, proxy, and Tor exit node detection with IP2Proxy and Spur

## What I Did

### 1. Domain enrichment

I learned what each source says about a domain:

- **A and AAAA records** show where it is hosted, and an IP behind a CDN range tells little by itself.
- **TXT records** show the mail security setup and the tools in use, while attacker domains often have empty records or suspicious SPF or faked DKIM.
- **WHOIS and RDAP** show the registrar and the domain's age, and malicious domains often live only days or a few months.

I also covered the attack techniques: CDN abuse to hide the real origin server, typosquatting such as look-alikes of a brand, and IDN attacks that swap in Cyrillic or Greek letters. The test for IDN is to convert the domain to Punycode, and a result that starts with `xn--` needs a closer look. In the practice alert I enriched a flagged domain using the exported reports to find its CDN and its age at the time of the alert.

### 2. IP enrichment

The simplest enrichment is to query the IP in AbuseIPDB and VirusTotal, and outside a CDN range even one detection is a sign of a risky IP. I also looked at three more data points:

- **ASN:** residential ASNs may point to VPN use or compromised consumer devices, hosting providers are the riskiest role because adversaries use them to distribute malware, and cloud and CDN ASNs need deeper analysis because both legitimate and malicious services use them.
- **Geolocation:** good for country-level checks on logons and traffic, but city-level data is unreliable.
- **VirusTotal comments:** can reveal what is hosted behind the IP.

In the practice alert for a suspected C2 address, I found its country, the C2 named in the comments, and the ASN with its tags.

### 3. Exposed services and certificates

Exposed services help from two sides: on a victim IP they show a likely way in (for example SSH), and on an attacker IP, outdated services or exposed RDP suggest a compromised host used as a jump point. I used Shodan and Censys exports to read open ports and service banners. Censys also shows services on non-standard ports. TLS certificates add more indicators: a self-signed issuer, a new or unusually long validity, and a subject that points to the software behind the HTTPS service. In the practice I identified an exposed remote access service, counted the open ports, found a service that leaked an active C2, and worked out the validity of its certificate.

### 4. VPN detection

A login at the expected location does not mean the user is real, because VPN services make faking a location trivial. IP2Proxy and Spur label VPN, proxy, and Tor exit nodes. I worked through two scenarios, one where the IP belongs to a VPN provider and one where it does not match any provider, to decide how to prioritise the alert.

### 5. Analyst workflow

The minimum workflow I follow for IP and domain intel:

- **Domain:** does it look legitimate or like typosquatting, what is its reputation and age, and which IP does it resolve to
- **IP:** is it a CDN range, what is its reputation, where is it located, is it a VPN node, and does the AS type match the alert

Some steps may be skipped or reordered depending on the alert.

### 6. Challenge

For a final challenge I gathered everything I could on a malicious domain used in an APT attack. Online tools returned little, as expected for short-lived infrastructure, so I used the exported incident report to find the IP it resolved to, the cloud provider, the server country, the domain creation date, and the operating system of the attack server.

## Result

I completed all the questions in the room, including the three practice alerts, the VPN scenarios, and the domain challenge.

## Takeaways

- Correlate signals from several sources instead of trusting one verdict.
- A new domain, a hosting-provider ASN, a self-signed certificate, and exposed remote access are common infrastructure red flags.
- CDN IPs say little by themselves, so the investigation moves to the domain and the content.
- A normal-looking location does not prove a legitimate user, because VPNs hide the real one.
- Threat intel data expires quickly, so I should save the evidence I find when I find it.
- Check for typosquatting and IDN tricks before trusting a look-alike domain.

I connected this to Intro to Cyber Threat Intel and File and Hash Threat Intel (the same verify, enrich, and decide process), and to the Phishing Analysis module, where suspicious domains and sender infrastructure were investigated.

## Skills

- Enriching domains with DNS records, WHOIS or RDAP age, and Punycode checks
- Recognising CDN abuse, typosquatting, and IDN attacks
- Checking IP reputation with AbuseIPDB and VirusTotal
- Reading ASN and geolocation data for triage decisions
- Using Shodan, Censys, and certificate data to understand an indicator's role
- Detecting VPN, proxy, and Tor traffic and judging alerts that involve them
- Following a repeatable IP and domain enrichment workflow
