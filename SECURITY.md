# Security Policy

## About This Repository

This repository contains **training materials only** — scenarios, guides, and exercises. It does not contain production credentials, live incident data, or real system configurations.

## Sensitive Data Policy

The following must **never** be committed to this repository:

- Real IOCs from live incidents (IP addresses, domains, hashes, URLs)
- SIEM queries containing real hostnames, usernames, or asset identifiers
- API keys, passwords, tokens, or any credentials
- Personally identifiable information (analyst names in tracking tables use placeholders)
- Internal network topology or asset inventory
- Screenshots of real alerts or dashboards

All scenario examples use fictional data: placeholder IPs (RFC 5737 — 198.51.100.0/24), fictional domains with [.] defanging, and generic usernames like `j.harris@[organisation].internal`.

## Reporting a Security Issue

If you find a process gap in these training materials that represents a real security risk:

1. Do not open a public GitHub issue
2. Contact the SOC Manager directly through the internal directory
3. Describe: the document, the gap, and the potential impact
4. Expect acknowledgement within 2 business days

## Supported Versions

Only the current `main` branch is maintained. Training materials are reviewed:
- After any major incident that reveals a gap
- When MITRE ATT&CK releases a major version update
- Annually as part of the training programme review

## Compliance Note

These training materials are reviewed to align with:
- NIST NICE Cybersecurity Workforce Framework
- SANS SOC curriculum
- Internal competency framework (maintained separately)
