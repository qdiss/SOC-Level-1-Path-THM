# Phishing Emails in Action - TryHackMe SOC Level 1

- **Module:** Phishing Analysis
- **Date completed:** 2026-11-10
- **Room link:** https://tryhackme.com/room/phishingemails2rytmuv
- **Path:** SOC Level 1

## Overview

This is the second room of the Phishing Analysis module. After learning
email structure and header analysis in the first room, I applied those
skills to real phishing samples. Each sample demonstrated a different
mix of social engineering tactics, such as brand impersonation, link
manipulation, tracking pixels, credential harvesting, and malicious
attachments.

## Objectives

- Identify common social engineering tactics used in phishing
- Analyse red flags in phishing emails
- Detect link manipulation and tracking pixels
- Deconstruct credential harvesting and attachment manipulation

## Tools / Concepts Covered

- Spoofed sender addresses and display name mismatches
- URL shorteners and redirect chain inspection (WhereGoes-style tools)
- Tracking pixels and why email providers block remote images
- Branded HTML templates for impersonation
- Multi-stage redirection and fake login portals
- BCC delivery, blank-body emails, and unusual attachment types
- Malicious Office attachments leading to executable downloads
- Defanging indicators when reporting them

## Analysis Process

### Step 1 - Spoofing and Shortened Links

The first sample imitated a payment service receipt. The sender
display name and the real sending address did not match, the
recipient address looked unusual, and the only interactive element was
a button pointing to a shortened URL. Because a shortener hides the
real destination, I learned to use redirect-tracing tools to see where
it leads without visiting it.

### Step 2 - Tracking Pixels and Link Manipulation

The second sample posed as a shipping notification with a fake
tracking number. Checking the raw source showed that the tracking
number link was manipulated and that the email contained a tiny
embedded image used as a tracking pixel, which tells the sender when
the message is opened. This explained why the mail provider blocked
images and disabled links in the message.

### Step 3 - Credential Harvesting Chain

The third sample used a chain of redirections through pages that
imitated well-known document-sharing and software brands before
reaching a fake sign-in page. The login always failed with a generic
error, because the page only forwards the entered credentials to the
attacker. Artificial urgency (a link that "expires today") was used to
push the user to act without thinking. I also noted that grammar and
formatting mistakes are less reliable indicators now that attackers
can generate polished text with AI.

### Step 4 - Malicious Attachments

Three samples relied on attachments instead of links: a PDF with an
embedded link leading to a non-legitimate domain, an unusual Office
template file attached to a blank email whose recipients were hidden
using BCC, and a spreadsheet whose single link attempted to download
and run an executable. In each case, mismatched display names,
inconsistent regional details, atypical formats, and urgency were the
warning signs. A successful payload could provide persistence, data
theft, or ransomware.

## Outcome / Classification

All samples were classified as phishing. For each one I identified the
techniques used, the mismatch between displayed and actual sender, and
the mechanism meant to deliver the malicious content.

## Key Takeaways

Most phishing emails combine a few simple tricks: a convincing sender
name, urgency, and a link or attachment that hides the real
destination. The reliable checks are the same each time: compare the
display name with the real address, inspect the raw source, trace
shortened URLs safely, and treat any unexpected attachment type as
suspicious. Because visual quality is no longer a dependable signal, I
should rely on technical evidence in the headers, links, and files
rather than on how professional the email looks.

## Skills Demonstrated

- Identifying sender spoofing and display name mismatches
- Tracing shortened URLs and redirect chains safely
- Recognising tracking pixels, credential harvesting pages, and malicious attachments
- Explaining the intent and potential impact of each phishing technique
- Defanging indicators when documenting findings
