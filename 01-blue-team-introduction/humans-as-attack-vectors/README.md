# Humans as Attack Vectors - TryHackMe SOC Level 1

- **Module:** Blue Team Introduction
- **Date completed:** 2026-09-23
- **Room link:** https://tryhackme.com/room/humansattackvectors
- **Path:** SOC Level 1

## Overview

This room looks at why humans are the weakest link in cyber security,
covers the main social engineering techniques attackers use to
manipulate people, and explores how a SOC both mitigates and detects
these human-targeted attacks - practiced through two hands-on
scenarios.

## Objectives

- Understand the role of the human element in cyber security
- Understand the SOC's role in detecting and mitigating human-targeted attacks
- Practice the concepts in two realistic scenarios

## Tools / Concepts Covered

- Social engineering principles (trust, emotional manipulation)
- Phishing, malware downloads, deepfakes, impersonation
- Mitigation vs. detection as complementary defense layers
- Security awareness training, anti-phishing tooling, EDR/antivirus
- SOC dashboard simulation ("Employees at Risk", "Security Policy" scenarios)

## Analysis Process

### Step 1 - Why Attackers Target Humans

The room frames it simply: breaching a technical defense is hard, but
tricking a person into handing over access is often much easier.
Attackers don't always target a specific person - some go after
whoever provides useful access (an HR manager's account for employee
data, an IT admin's VPN credentials for network-wide access), while
others breach broadly and decide what to do with the access later.

### Step 2 - Social Engineering Techniques

The core techniques covered were phishing (fake login pages harvesting
credentials), malware disguised as legitimate downloads (fake CAPTCHAs,
malicious QR codes, SEO poisoning), deepfake audio/video used to
impersonate executives or colleagues, and straightforward impersonation
over phone/email (e.g., someone posing as IT support). The common
thread across all of them is exploiting trust and urgency rather than
a technical flaw.

### Step 3 - Mitigation vs. Detection

The room draws a clear line between the two: mitigation reduces the
chance or impact of an attack before it happens (anti-phishing tools,
EDR/antivirus, security awareness training), while detection is the
SOC's job for catching what slips past mitigation. This reframed how I
think about the SOC analyst role - it's not just about analyzing
alerts, but also about feeding back mitigation improvements when
patterns keep showing up.

### Step 4 - Practical Scenarios

The hands-on lab simulated being a SOC analyst at a company dashboard
with two parts: identifying and acting on "Employees at Risk" (likely
users being actively targeted or compromised), and reviewing/adjusting
a "Security Policy" tab to reduce exposure going forward - combining
the detection and mitigation sides covered earlier in one exercise.

## Outcome / Classification

Completed both practical scenarios ("Employees at Risk" and "Security
Policy") successfully.

## Key Takeaways

This room reinforced that most real-world breaches start with a person,
not a firewall bypass - which changes what I should be watching for as
an analyst: unusual login behavior, reported suspicious emails, and
requests that create false urgency. It also showed me that a good SOC
analyst doesn't just detect these attacks but also proposes mitigation
improvements (training, tooling) to reduce how often they succeed in
the first place.

## Skills Demonstrated

- Recognizing common social engineering techniques (phishing, deepfakes, impersonation)
- Distinguishing mitigation vs. detection responsibilities in a SOC
- Applying human-risk awareness in a simulated SOC dashboard scenario
