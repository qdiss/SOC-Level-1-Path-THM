# Risk Assessment - TryBike Parts (fictional online shop)

> Sample document created for learning purposes. TryBike Parts is a
> fictional online shop selling bicycle parts. Drafted with AI
> assistance and reviewed and edited by the author.

## Scope

The public website and web application, the customer database, the
payment integration, and employee accounts.

## Method

Each risk is rated for **likelihood** and **impact** on a 1-3 scale
(1 = low, 3 = high). The rating is likelihood multiplied by impact:
1-2 low, 3-4 medium, 6-9 high.

## Risk Register

| # | Risk | Likelihood | Impact | Rating | Mitigation | Owner |
|---|------|-----------|--------|--------|------------|-------|
| 1 | Phishing leads to employee account compromise | 3 | 3 | 9 (High) | Security awareness training, anti-phishing email filtering, MFA | IT / Security |
| 2 | Application-layer DDoS makes the website unavailable | 2 | 3 | 6 (High) | CDN, WAF rate limiting, CAPTCHA challenges, input validation | IT / DevOps |
| 3 | SQL injection exposes customer data | 2 | 3 | 6 (High) | Parameterised queries, input validation, WAF, regular testing | Development |
| 4 | Unpatched web server or dependencies exploited | 2 | 3 | 6 (High) | Patch management process, dependency scanning | IT / DevOps |
| 5 | Web shell uploaded through weak file upload validation | 1 | 3 | 3 (Medium) | Strict upload validation, least privilege, file integrity monitoring | Development |
| 6 | Weak or reused passwords on admin accounts | 2 | 2 | 4 (Medium) | Password policy, MFA, account reviews | IT / Security |
| 7 | Sensitive data pasted into public AI tools | 2 | 2 | 4 (Medium) | AI use policy, staff guidance, approved tools only | Security / HR |
| 8 | Insufficient logging delays incident detection | 2 | 2 | 4 (Medium) | Central log collection in a SIEM, alerting on key events | Security |

## Priority Actions

1. Enforce MFA and phishing protection for all employee accounts.
2. Put the site behind a CDN and WAF with rate limiting.
3. Review code for injection and upload weaknesses and keep dependencies patched.
4. Centralise web server logs in a SIEM and define alerts.

## Review Notes (author)

Points I checked in the AI-generated draft before keeping it:

- **Ratings are assumptions.** The likelihood and impact values are
  estimates for a fictional shop, not measured data. I kept the simple
  1-3 scale so the logic is easy to follow, but a real assessment would
  base the ratings on asset value, incident history, and threat
  intelligence.
- **Risk coverage.** The register covers the main areas of the scope
  (accounts, the website, the customer database, and logging). I
  included the risk of sensitive data being entered into public AI
  tools, because that is a realistic new risk and is addressed in the
  acceptable use policy.
- **Mitigations are consistent with the policy.** Controls such as MFA,
  security awareness training, and logging in a SIEM match the rules in
  the acceptable use policy, so the two documents support each other.
- **Mitigations are general.** Each one names a control but not a
  specific product, deadline, or cost. A real plan would add those and
  confirm that the owners have the capacity to deliver.
- **No legal check.** The documents do not cite specific laws or
  standards in detail. Anything legally binding, such as GDPR
  obligations, would need review by a compliance or legal specialist
  and verification against the official text, not the AI draft.

## Limitations

This is a simplified exercise with assumed values. A real assessment
would use the organisation's actual assets, threat data, and agreed
risk criteria.
