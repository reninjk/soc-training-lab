# Contributing to soc-training-lab

Thank you for improving the SOC training programme. This repository contains onboarding guides, exercise scenarios, tabletop exercises, and certification resources.

## What Belongs Here

- Analyst onboarding guide updates
- New training scenarios (phishing, malware, lateral movement, etc.)
- Tabletop exercise scripts and inject variants
- SIEM query cheatsheet additions
- Certification roadmap updates (new certs, updated study times)
- Quick-reference cards for tools and procedures

## What Does Not Belong Here

- Real incident data, actual IOCs, or SIEM query results from live incidents
- Internal system names, hostnames, or user accounts
- Personally identifiable information (analyst names, contact details)
- Credentials, API keys, or access tokens of any kind

## Contribution Process

1. **Open an issue** describing the new scenario or update
2. Branch: `feat/scenario-name`, `fix/doc-title`, or `docs/section`
3. Submit a PR using the PR template
4. Scenarios require review by a senior analyst or SOC Manager
5. New tabletop exercises require SOC Manager approval before merging

## Scenario Quality Bar

All scenarios must include:
- Clear learning objectives
- MITRE ATT&CK technique references
- Realistic but fictional data (no real IOCs)
- Phase structure with time estimates
- Debrief questions
- Trainer notes with difficulty variants

## Commit Messages

```
feat: add scenario-02 BEC wire fraud triage
docs: update cert roadmap with GIAC GREM
fix: correct KQL syntax in siem-query-cheatsheet
```

## Questions

Open a GitHub Discussion or raise a ticket with the SOC training coordinator.
